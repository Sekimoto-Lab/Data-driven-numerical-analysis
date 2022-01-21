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

### Lorenzクラスを作成
```Python
class Lorenz():
    def __init__(self, s=10.0, r=8/3, b=33.5, t=0.001, iter=500):
        ## パラメータの設定
        self.s = s
        self.r = r
        self.b = b

        ## 時間刻み
        self.t = t

        ##イテレーション
        self.iter = iter

    def main(self, X_0):
        '''
        Input X_0
        --------------
        X_0 : np.array([batch_size * 3])

        Return X, Y, Z
        -------------
        x : np.array([iter * batch_size])
        y : np.array([iter * batch_size])
        z : np.array([iter * batch_size])
        '''
        ## 初期値を取得
        x_0 = X_0[:,0] 
        y_0 = X_0[:,1]
        z_0 = X_0[:,2]

        ## x, y, z を格納する行列を作成
        x = np.array([x_0])
        y = np.array([y_0])
        z = np.array([z_0])

        ##計算を実行とx,y,zに格納
        for i in tqdm(range(1,self.iter)):
            if i==1:  # オイラー法
                next_x = x + self.s * (y - x) * self.t
                next_y = y + (x * (self.r - z) - y) * self.t
                next_z = z + (x * y - self.b * z) * self.t
            else:
                next_x, next_y, next_z = self.calc(x, y, z)

            x = np.vstack((x, next_x))
            y = np.vstack((y, next_y))
            z = np.vstack((z, next_z))
        
        ## インスタンス化
        self.x = x; self.y = y; self.z = z

        ## バッチサイズの取得
        self.batch = X_0.shape[0]

    def calc(self, x, y, z):
        next_x = x[-1] + (3/2 * self.s * (y[-1] -x[-1]) - 1/2 * self.s * (y[-2] - x[-2])) * self.t
        next_y = y[-1] + (3/2 * (x[-1] * (self.r - z[-1]) - y[-1]) - 1/2 * (x[-2] * (self.r - z[-2]) - y[-2])) * self.t
        next_z = z[-1] + (3/2 * (x[-1] * y[-1] - self.b * z[-1]) - 1/2 * (x[-2] * y[-2] - self.b * z[-2])) * self.t
        return next_x, next_y, next_z


    def show_3d_2d(self):
        ## 3D
        plt.style.use('ggplot')
        plt.rcParams['axes.facecolor'] = 'white'

        fig = plt.figure(figsize=(20,10))
        ax_3d = fig.add_subplot(1,2,1,projection='3d')

        for i in range(self.batch):
            ax_3d.plot(self.x[:,i], self.y[:,i], self.z[:,i], label=f'Lorentz curve {i}')
        ax_3d.legend()

        ## 2D
        ax_2d_1 = fig.add_subplot(3,2,2)
        for i in range(self.batch):
            ax_2d_1.plot(self.x[:,i], label=f'Lorentz curve {i}')
            ax_2d_1.set_ylabel('x', fontsize=20)

        ax_2d_2 = fig.add_subplot(3,2,4)
        for i in range(self.batch):
            ax_2d_2.plot(self.y[:,i], label=f'Lorentz curve {i}')
            ax_2d_2.set_ylabel('y', fontsize=20)

        ax_2d_3 = fig.add_subplot(3,2,6)
        for i in range(self.batch):
            ax_2d_3.plot(self.z[:,i], label=f'Lorentz curve {i}')
            ax_2d_3.set_ylabel('z', fontsize=20) 
```

```Python
## パラメータの設定
lorenz1 = Lorenz(s=10.0, r=33.5, b=8/3, t=0.001, iter=30000)

## 初期値の設定 (バッチ×3変数)
X_0 = np.array([[1.0,1.0,1.0],
                [1.01,1.1,1.1],
                [0.99,0.99,0.99]])
## 計算実行
lorenz1.main(X_0)

## 描写
lorenz1.show_3d_2d()
```
