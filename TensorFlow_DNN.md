# TensorFlow sample: Deep neural network for MNIST (TensorFlow v1 compatible を利用) 
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

- どのようなデータかを確認しておく (trainが学習用データ，testが検証用データ． Xが画像データ,　Yが正解ラベル(one-hot形式.) 
```
plt.imshow(testX[1].reshape(28,28), cmap=cm.gray, interpolation='nearest')
plt.show()
```

## ディープ・ニューラルネットワーク(DNN)の作成
```Python
import tensorflow as tf

# construct a neural-network
tf.compat.v1.reset_default_graph()

# input layer
net = tflearn.input_data(shape=[None,784])

# hidden layer 
net = tflearn.fully_connected(net, 128, activation='relu')
net = tflearn.dropout(net, 0.5) # 50 % を残す

# output layer
net = tflearn.fully_connected(net, 10, activation='softmax') # return 0--1 probability
#net = tflearn.regression(net, optimizer='sgd', learning_rate=0.5, loss='categorical_crossentropy')
net = tflearn.regression(net, optimizer='adam', loss='categorical_crossentropy')
```

## 学習モデルの作成
```Python
# training 
model = tflearn.DNN(net)
model.fit(trainX,trainY, n_epoch = 20, batch_size=100, validation_set=0.1, show_metric=True)
```

## 予測モデルの結果を確認(testXが画像データ, predが予測結果)
```Python
# prediction
pred = np.array(model.predict(testX)).argmax(axis=1)
print(pred)
```
- 全データでモデルの精度をチェック (testYがone-hot形式なので， 正解の数字(label)に変換)
```Python
print(testY.shape)
label = testY.argmax(axis=1)
print(label)

accuracy = np.mean(pred==label, axis=0)
print(accuracy)
```

(参考) 
「初めてのTensorFlow」 足立悠 著 (リックテレコム)
