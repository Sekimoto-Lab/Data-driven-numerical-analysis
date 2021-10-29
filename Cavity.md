## Cavity flow by Fractional step method

```Python
import numpy as np 
import matplotlib.pyplot as plt # 図の作成環境のロード
from matplotlib import cm # カラーマップ
from tqdm import tqdm # プログレスバーを表示
import time # 計算時間計測プロファイリング用
```

各種パラメターの設定
```Python
# parameters 
# computational domain
Lx=0.1e0
Ly=Lx

# wall velocity
Uwall=0.01
# fluid property
nu=1.e-6 # kinematic viscosity (=mu/rho)
# rho=1.e3 # density

# nondimensional time (in L/Uwall)
endT=20

# mesh girds
Nx=41
Ny=43

# for dt
CFL=0.5
CFLv=0.8

# for solver
accel = 1.925e0
err_tol = 1.e-6
tiny = 1.e-20
```

メッシュと座標の設定
```Python
# set grid 
dx=Lx/np.float64(Nx-1)
dy=Ly/np.float64(Ny-1)

# mesh information (grid)
x=np.array(np.zeros(Nx+2),dtype=np.float64)
y=np.array(np.zeros(Ny+2),dtype=np.float64)
# cell centre position
xc=np.array(np.zeros(Nx+1),dtype=np.float64)
yc=np.array(np.zeros(Ny+1),dtype=np.float64)

# set uniform mesh
x[0]=-dx
for i in range(1,Nx+2):
  x[i]=x[i-1]+dx; # raw grid
  xc[i-1]=0.5*(x[i-1]+x[i])/Lx # scaled axis of cell centre

y[0]=-dy
for i in range(1,Ny+2):
  y[i]=y[i-1]+dy; # raw grid 
  yc[i-1]=0.5*(y[i-1]+y[i])/Lx # scaled axis of cell centre

x2d, y2d = np.meshgrid(x,y) # for vector plots
```

時間刻みの設定をCFD条件から行う．
```Python
if Uwall != 0.0:
  Uref = Uwall
else:
  Uref = nu/Lx
    
dt = min(CFL*dx/Uref, CFLv*dx*dx/nu)
Nt = int(endT*Lx/Uref/dt)

dx2=(dx*dx) 
dy2=(dy*dy)
```

変数（速度(u,v), 圧力(p))の配列を定義
```Python
# variables 
u=np.zeros([Ny+1, Nx+2],dtype=np.float64)
v=np.zeros([Ny+2, Nx+1],dtype=np.float64)
p=np.zeros([Ny+1, Nx+1],dtype=np.float64)

uaux=np.zeros([Ny+1, Nx+2],dtype=np.float64)
vaux=np.zeros([Ny+2, Nx+1],dtype=np.float64)
dive=np.zeros([Ny+1, Nx+1],dtype=np.float64)
    
# for plot the velocity on regular grid
ur=np.array(np.zeros((Ny, Nx+2),dtype=np.float64))
vr=np.array(np.zeros((Ny+2, Nx),dtype=np.float64))
```

x方向の速度の更新用の関数
```

```
