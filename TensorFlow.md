## TensorFlow sample DNN for MNIST (using TensorFlow v1 mode) 

MNISTサンプル画像データを使った文字認識のDNNプログラムを作成する．

- はじめに，MINISTデータのダウンロード (Colaboratory を使う場合, 以下のようにGoogle　Driveのアカウント上のドライブをマウントして使う）
```Python
from google.colab import drive
drive.mount('/content/drive')
import os
os.chdir('/content/drive/MyDrive/Colab Notebooks/')
os.getcwd()
```
- MINISTデータを保存するフォルダを ./data/minist とする．
```Python
!mkdir data
!mkdir data/mnist
```

- TensorFlow 
```Python
#!/usr/bin/env python3
!pip install tflearn

import tensorflow as tf
import tflearn 
import tflearn.datasets.mnist as mnist 

from matplotlib import pyplot as plt
from matplotlib import cm
import numpy as np 
```

