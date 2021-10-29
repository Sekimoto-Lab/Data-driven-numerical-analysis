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

