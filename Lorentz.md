# Lorentz model by RK method (学生作品①)

### ライブラリのインポートのおまじない
```Python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
```
### 初期値の設定
```Python
dt = 0.01
t_0 = 0
t_1 = 50
X_0 = np.array([1, 1, 1])
```

### ルンゲ・クッタ法で時間発展させる関数の設定
```Python
def RungeKutta(x0, y0, z0, t, X):
  k_1 = LorenzEquation(x0, y0, z0, t, X)
  k_2 = LorenzEquation(x0, y0, z0, t + dt/2, X + k_1*dt/2)
  k_3 = LorenzEquation(x0, y0, z0, t + dt/2, X + k_2*dt/2)
  k_4 = LorenzEquation(x0, y0, z0, t + dt, X + k_3*dt)
  X_next = X + dt/6*(k_1 + 2*k_2 + 2*k_3 + k_4)

  return X_next

def LorenzEquation(x0, y0, z0, t, X):
  x = X[0]
  y = X[1]
  z = X[2]

  return np.array([-x0*x + x0*y, -x*z + z0*x - y, x*y - y0*z])

def Lorenz(x0, y0, z0):
    t = t_0
    X = X_0
    data = np.r_[X]

    while t < t_1:
        X = RungeKutta(x0, y0, z0, t, X)
        t += dt
        data = np.c_[data, X]
        
    return data
```

### 実行
```python
[X,Y,Z]=Lorenz(10, 8/3, 33.5)
```

### 図のプロット        
```python
[X,Y,Z]=Lorenz(10, 8/3, 33.5)

fig = plt.figure()
ax = Axes3D(fig)
ax.plot(X, Y, Z)
plt.show()
```

# batched Lorentz model (学生作品②， アダムス・バッシュホース法)

### おまじない
```Python
import numpy as np
import matplotlib.pyplot as plt
from tqdm import tqdm
```


$i=0$のとき，
$$x_0 = x_0, y_0 = y_0, z_0 = z_0$$

$i=1$のとき，(普通のニュートン法)
\begin{align*}
x_1 &= x_0 + \{\sigma (y_0 - x_0)\} dt \\
y_1 &= y_0 + \{x_0(Ra - z_0) - y_0\} dt \\
z_1 &= z_0 + \{x_0 y_0 - \beta z_0\} dt
\end{align*}

$i=2,...$のとき，(アダムス・バッシュフォース法)
\begin{align*}
x_{i+1} &= x_i + \left[\frac{3}{2}\sigma(y_i-x_i) - \frac{1}{2}\sigma(y_{i-1}-x_{i-1})\right] dt \\
y_{i+1} &= y_i + \left[\frac{3}{2}\{x_i(Ra-z_i)-y_i\} - \frac{1}{2} \{x_{i-1}(Ra-z_{i-1})-y_{i-1}\}\right] dt \\
z_{i+1} &=  z_i + \left[\frac{3}{2}(x_iy_i-\beta z_i) - \frac{1}{2}(x_{i-1}y_{i-1} - \beta z_{i-1}) \right] dt
\end{align*}

