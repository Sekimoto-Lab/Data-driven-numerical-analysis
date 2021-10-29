## Support page for Data-driven numerical analysis & Informational environmental modeling 

### 1D heat transfer (Python sample code)

はじめに，Pythonのはじめのおまじない（数値解析用のnumpyライブラリと図示のためのmatplotlibを使う） 
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

時間刻みを設定（安定条件に注意)
```Python
dt=0.001; # set from dt/(dz*dz) < 1/2
dtz2 = dt/dz/dz
```

全要素を更新しするループ関数の例
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
```
# time loop
for itr in range(1,20):
  loop_z(T, T0, Tbc0, Tbc1)
  T0 = T.copy();
# plot 
plt.plot(zp, T, marker='o');
plt.xlim([0, 1]); plt.ylim([0, 1]); plt.show()
```

### 2D heat transfer

1. Governing equations & Boundary conditions
2. Python sample code

### 2D Cavity flow [Link (GitHub source code)](https://github.com/Sekimoto-Lab/Cavity)

1. Governing equations & Boundary conditions
2. Python sample code

---
### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```


Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Sekimoto-Lab/Data-driven-numerical-analysis/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

[Atsushi Sekimoto Lab. Homepage](https://sites.google.com/view/sekimoto-lab/lectures/%E3%83%87%E3%83%BC%E3%82%BF%E9%A7%86%E5%8B%95%E5%9E%8B%E6%95%B0%E5%80%A4%E8%A8%88%E7%AE%97-data-driven-numerical-analysis)
