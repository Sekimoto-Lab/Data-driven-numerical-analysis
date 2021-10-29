はじめに，Pythonのおまじない（数値解析用のnumpyライブラリと図示のためのmatplotlibを使う） 
```Python
import numpy as np 
import matplotlib.pyplot as plt 
```

続いて，微小要素に分けて，それぞれの要素の温度の初期値と，境界条件の設定．
```Python
Nz=20; 
T0 = np.zeros(Nz+1); T = np.zeros(Nz+1);

T0[:] = 1.0; Tbc0 = 0.0;  Tbc1 = 0.0 

T0[0] = Tbc0; # left edge
T0[Nz]= Tbc1; # right edge
```

次に，棒の長さと要素中心の座標と要素の端の座標を定義．
```Python
L=1.0; 
dz = L/Nz; z = np.linspace(0.0, L, Nz); zp= np.linspace(-0.5*dz, L+0.5*dz, Nz+1)
```

また，時間刻みを設定（安定条件に注意)
```Python
dt=0.001; # set from dt/(dz*dz) < 1/2
dtz2 = dt/dz/dz
```

全要素の温度T0をTを更新するループ関数の例
```Python
def loop_z(T, T0, Tbc0, Tbc1):
  for i in range(T0.size):
    if i==0:
      T[i] = 2.0*Tbc0 - T0[i+1]
    elif i==T0.size-1:
      T[i] = 2.0*Tbc1 - T0[i-1]
    else: 
      T[i] = (1.0 - 2.0*dtz2)*T0[i] + dtz2*(T0[i+1] + T0[i-1]); 
  return
```

計算実行(20ステップの例) 
配列のコピーにはcopy()を使う． copyのあとのかっこ()を忘れないように!
```
# time loop
for itr in range(1,20):
  loop_z(T, T0, Tbc0, Tbc1)
  T0 = T.copy();
# plot 
plt.plot(zp, T, marker='o');
plt.xlim([0, 1]); plt.ylim([0, 1]); plt.show()
```
