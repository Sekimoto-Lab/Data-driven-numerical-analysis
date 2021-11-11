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

```Python
# for dt
CFL=0.5
CFLv=0.8
if itemp==1:
    CFLk=CFLv
```
