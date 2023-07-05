## 2D thermal cavity flow (Python code)  

2D Cavity Flow のプログラムにエネルギー方程式を追加する.

追加パラメターとして Fluid property に以下を追加．
(itempというスイッチングフラグを立てて，エネルギー方程式を解くかどうかを切り替えるコーディングスタイルとした．）
```Python
# thermal property 
itemp = 1
if itemp==1:
    Tbc_H = 1.0 # 左壁面の温度
    Tbc_L = 0.0 # 右壁面の温度
    #Tref=Tbc_H - Tbc_L # the code assumes that Tref=1.0 
    kappa=1.e-6
    gbeta =1.e-3 # gravity coeff, thermal expansion rate
```

高Pr流体の場合は熱伝導支配となるので，それに備える．
```Python
# for dt
CFL=0.5
CFLv=0.8
if itemp==1:
    CFLk=CFLv
```

代表速度については，上壁面の速度とする．上壁面が動かない場合(Uwall=0)は自然対流(重力)による代表速度を考える．
```Python
if Uwall != 0.0:
    # Uref = Uwall
    Uref = Uwall + sqrt(gbeta*Lx) # こちらの方がより安定 (2023-07-05 sekimoto) 
elif gbeta != 0.0:
    Uref = np.sqrt(gbeta*Lx) 
else:
    Uref = nu/Lx
    
dt = min(CFL*dx/Uref, CFLv*dx*dx/nu)
if itemp==1:
    dt = min(dt, CFLk*dx*dx/kappa, CFL*dy/np.sqrt(gbeta*Lx)) # Lx is added, 2021/07/09
```

変数variablesに温度関連の配列を追加．
```Python 
if itemp==1:
    T   =np.zeros([Ny+1, Nx+1],dtype=np.float64)
    Taux=np.zeros([Ny+1, Nx+1],dtype=np.float64)
```

温度に関する関数(calc_temp, set_bc_temp, add_grav)を追加． 重力はy方向のみ考慮．(一部修正 T --> Taux, 2023-07-05 sekimoto)
```Python
def calc_temp(T,Taux,u,v):
    for jc in range(1,Ny):
        for ic in range(1,Nx):
            tdiff = (Taux[jc, ic-1]-2e0*Taux[jc,ic]+Taux[jc, ic+1])/dx2 \
                    +(Taux[jc-1, ic]-2e0*Taux[jc,ic]+Taux[jc+1, ic])/dy2
            tconv_x = ( ( u[jc, ic]*(-Taux[jc, ic-1]+Taux[jc, ic])/dx ) \
                               +( u[jc, ic+1]*(-Taux[jc, ic]+Taux[jc, ic+1])/dx ) \
                               )/2e0             
            tconv_y = (  (v[jc, ic]*(-Taux[jc-1,ic]+Taux[jc, ic])/dy ) \
                                 +(v[jc+1,ic]*(-Taux[jc,ic]+Taux[jc+1,ic])/dy ) \
                                )/2e0
            T[jc][ic]= Taux[jc][ic] -dt*(tconv_x + tconv_y) + dt*kappa*tdiff 
# ---------------------------------------------- #
def set_bc_temp(T, Tbc_H, Tbc_L):
    for jc in range(1, Ny):
        T[jc, 0] = 2.0*Tbc_H - T[jc, 1]     # left imaginary cell
        T[jc, Nx] = 2.0*Tbc_L - T[jc, Nx-1] # right imaginary cell

    # bottom and top walls
    for ic in range(1, Nx):
        T[0, ic] = T[1, ic]     # bottom wall, adiabatic 
        T[Ny, ic] = T[Ny-1, ic] # moving wall, adiabatic
# ---------------------------------------------- #
# Buoyancy force (Boussinesq approximation) in y
def add_grav(vaux,T):
    for j in range(1, Ny):
        for ic in range(1, Nx):
            vaux[j, ic] = vaux[j, ic] + dt*gbeta*((T[j,ic] + T[j+1,ic])/2.0 )
```
 
## 以上で部品は揃ったので， メインループ内部の改造は各自で演習．
