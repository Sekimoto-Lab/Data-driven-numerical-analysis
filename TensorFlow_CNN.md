# TensorFlow sample: Convolutional Deep neural network for MNIST (TensorFlow v1 compatible を利用) 
MNISTサンプル画像データを使った文字認識のCNNプログラムを作成する演習．

- はじめに， Pythonのおまじない
```Python
# 画像のプロット用
from matplotlib import pyplot as plt
from matplotlib import cm
import numpy as np
```

- MINISTデータのダウンロード先のフォルダを作成しておく. Colaboratory を使う場合, 以下のようにGoogle　Driveのアカウント上のドライブをマウントして使う. 
[A sample deep neural network (TensorFlow)](TensorFlow.md) 参照


- MINISTのデータをロードする． (初めての場合は，自動的にデータがダウンロードされる．)
```Python
# MNIST data setを扱うためのライブラリ 
# tflearnのインストールが必要 (!pip install tflearn)
!pip install tflearn
import tflearn 
import tflearn.datasets.mnist as mnist
# download MNIST data and load
trainX, trainY, testX, testY = mnist.load_data('./data/mnist',one_hot=True) 
```

- データを2次元に変換
```Python
trainX = trainX.reshape([-1, 28, 28, 1])  # -1でその要素は良きに計らう．
testX = testX.reshape([-1, 28, 28, 1])
```


# TensorFlowのライブラリをインポート
```Python
import tensorflow as tf

# CNN 用のライブラリ
from tflearn.layers.core import input_data, dropout, fully_connected
from tflearn.layers.conv import conv_2d, max_pool_2d
from tflearn.layers.normalization import local_response_normalization
from tflearn.layers.estimator import regression

```

# 畳み込みニューラルネットワークの作成
```Python
# construction of CNN
# initialization
tf.compat.v1.reset_default_graph()

# input layer
net = input_data(shape=[None, 28, 28, 1]) # grayscale data なので 1, カラーならば ..., 
```

```Python
# hidden layer (中間層) = convolution + ReLU + pooling
# convolution sublayer
net = conv_2d(net, 32, 5, activation='relu') # 32ノード # 5 フィルターサイズ (5x5), サイズが変わらないようにゼロパディングが暗になされる
# pooling
net = max_pool_2d(net, 2) # 2x2 の中の最大値をとる

# 2nd layer
net = conv_2d(net, 64, 5, activation='relu')
net = max_pool_2d(net, 2)

# 全結合層
net = fully_connected(net, 64, activation='relu')
net = dropout(net, 0.5) # けっこう重要
```

```Python
# output layer 
net = tflearn.fully_connected(net, 10, activation='softmax')
net = tflearn.regression(net, optimizer='sgd', learning_rate=0.5, loss='categorical_crossentropy') 
#net = tflearn.regression(net, optimizer='adam', loss='categorical_crossentropy') 
# momentum, adam 
```

# 学習の実行
```Python 
# learning 
model = tflearn.DNN(net)
model.fit(trainX, trainY, n_epoch=20, batch_size=100, validation_set=0.1, show_metric=True)
```

# 結果のチェック
- testデータを判別
```Python
# prediction
pred = np.array(model.predict(testX)).argmax(axis=1)
print(pred)

# check the answer
label = testY.argmax(axis=1)
print(label)
```

- 全体の精度を確認
```Python
accuracy = np.mean(pred == label, axis=0)
print(accuracy)
```

