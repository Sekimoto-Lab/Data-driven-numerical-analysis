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
- MINISTデータを保存するフォルダを ./data/minist とする． notebook環境からターミナルコマンドを実行するには，エクスクラメーションマーク（!）をつける (mkdir はフォルダを作成するコマンド)． フォルダの作成はWindowsエクスプローラーなどで作成しても良い． 
```Python
!mkdir data
!mkdir data/mnist
```

- TensorFlow tfleanライブラリを読み込みMINISTのデータをダウンロードする．
```Python
#!/usr/bin/env python3
!pip install tflearn
import tflearn 
import tflearn.datasets.mnist as mnist 
trainX, trainY, testX, testY = mnist.load_data('./data/mnist',one_hot=True)
```

```Python
import tensorflow as tf
from matplotlib import pyplot as plt
from matplotlib import cm
import numpy as np 
```


