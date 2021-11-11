## 2D thermal cavity flow (Python code)  

2D Cavity Flow のプログラムにエネルギー方程式を追加する.

追加パラメターとして Fluid property に以下を追加．
```Python
# thermal property 
itemp = 1
if itemp==1:
    Tbc_H = 1.0 # 左壁面の温度
    Tbc_L = 0.0 # 右壁面の温度
    #Tref=Tbc_H - Tbc_L # the code assume that Tref=1.0 
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

代表速度については，上壁面の速度とする．動かない場合は自然対流(重力)による代表速度を考える．
```Python
if Uwall != 0.0:
    Uref = Uwall
elif gbeta != 0.0:
    Uref = np.sqrt(gbeta*Lx) 
else:
    Uref = nu/Lx
    
dt = min(CFL*dx/Uref, CFLv*dx*dx/nu)
if itemp==1:
    dt = min(dt, CFLk*dx*dx/kappa, CFL*dy/np.sqrt(gbeta*Lx)) # Lx is added, 2021/07/09
```
