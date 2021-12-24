# TensorFlow sample: Convolutional Deep neural network for MNIST (TensorFlow v1 compatible を利用) 
MNISTサンプル画像データを使った文字認識のCNNプログラムを作成する演習．

- はじめに， Pythonのおまじない
```Python
import tensorflow as tf
import tflearn 

# CNN 用のライブラリ
from tflearn.layers.core import input_data, dropout, fully_connected
from tflearn.layers.conv import conv_2d, max_pool_2d
from tflearn.layers.normalization import local_response_normalization
from tflearn.layers.estimator import regression

# MNIST data setを扱うためのライブラリ 
import tflearn.datasets.mnist as mnist

# 画像のプロット用
from matplotlib import pyplot as plt
from matplotlib import cm
import numpy as np
```

- MINISTデータのダウンロード先のフォルダを作成しておく. Colaboratory を使う場合, 以下のようにGoogle　Driveのアカウント上のドライブをマウントして使う. 


- MINISTのデータをロードする． (初めての場合は，自動的にデータがダウンロードされる．)
```Python
# download MNIST data and load
trainX, trainY, testX, testY = mnist.load_data('./data/mnist',one_hot=True) 
```
