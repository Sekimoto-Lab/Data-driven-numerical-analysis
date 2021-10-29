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

x2d, y2d = np.meshgrid(x,y)    
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

