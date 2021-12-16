## TensorFlow sample DNN for MNIST (using TensorFlow v1 mode) 

MNISTサンプル画像データを使った文字認識のDNNプログラムを作成する．

- はじめに，MINISTデータのダウンロード (Colaboratory を使う場合, 以下のようにGoogle　Driveのアカウント上に保存しておくと良い）
```Python
!pip install tflearn
from google.colab import drive
drive.mount('/content/drive')
import os
```

- 
