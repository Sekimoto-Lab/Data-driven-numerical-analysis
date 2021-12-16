# TensorFlow sample DNN for MNIST (TensorFlow v1 compatible を利用．) 
MNISTサンプル画像データを使った文字認識のDNNプログラムを作成する演習．

- はじめに， Pythonのおまじない
```Python
#!/usr/bin/env python3
from matplotlib import pyplot as plt
from matplotlib import cm
import numpy as np 
```

- MINISTデータのダウンロード先のフォルダを作成しておく. Colaboratory を使う場合, 以下のようにGoogle　Driveのアカウント上のドライブをマウントして使う.
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

- TensorFlow tfleanライブラリを読み込み, MINISTのデータをロードする． (初めての場合は，自動的にデータがダウンロードされる．)
```Python
!pip install tflearn
import tflearn 
import tflearn.datasets.mnist as mnist 
trainX, trainY, testX, testY = mnist.load_data('./data/mnist',one_hot=True)
```

- どのようなデータかを確認しておく (one-hot形式) 
```
plt.imshow(testX[1].reshape(28,28), cmap=cm.gray, interpolation='nearest')
plt.show()
```

## ここからTensorflow を用いて, DNNの作成
```Python
import tensorflow as tf

# construct a neural-network
tf.compat.v1.reset_default_graph()

# input layer
net = tflearn.input_data(shape=[None,784])
```
```Python
# hidden layer 
net = tflearn.fully_connected(net, 128, activation='relu')
net = tflearn.dropout(net, 0.5) # 50 % を残す

# output layer
net = tflearn.fully_connected(net, 10, activation='softmax') # return 0--1 probability
#net = tflearn.regression(net, optimizer='sgd', learning_rate=0.5, loss='categorical_crossentropy')
net = tflearn.regression(net, optimizer='adam', loss='categorical_crossentropy')
```

