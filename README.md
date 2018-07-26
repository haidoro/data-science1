# Data Science1

コードはjupyter形式で保存したものです。
GitHub優れもので、jupyter形式のファイルも表示してくれます。

## SIGNATE登録

様々な練習問題のデータがあるので利用して他人のデータ分析と比較できたりする。

1. SIGNATE（旧DeepAnalytics）へ会員登録
2. case1というフォルダと、case2というフォルダを新規作成
3. SIGNATEのTOPページにある練習問題を探してダウンロードします。

***練習問題のリンクをクリックすると、概要が表示されます。さらに、データのページへ移動するとダウンロードできるようになっています。***

「【練習問題】お弁当の需要予測」と「【練習問題】銀行の顧客ターゲティング」のそれぞれからデータをダウンロードします。

お弁当の需要予測からダウンロードしたデータをcase1に、銀行の顧客ターゲティングからダウンロードしたデータをcase2に移動します。

## jupyterの使い方

* jupyterの使用方法は [Pythonの勧め（1）Jupyter Notebook（IPython Notebook）の使い方](https://itstudio.co/2018/01/19/7410/) を参照してください。

## pythonで簡単なデータ分析

1. データの読み込みとデータ閲覧
2. データの詳細を閲覧
3. グラフを描く
4. 欠損値を確認
5. 相関関係を調べる

基礎分析とは、データの不足を補ったり、間違ったデータを省く

## Lesson1 Pandasを使ったデータの読み込み

### ライブラリの読み込み

import文は次のようにします。
```
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
%matplotlib inline
```

### pandasを使ってデータ表示

Pandasは外部のデータをPythonに取り込み、表として表示してくれたり集計したりと非常に便利なライブラリです。
  
今回はまず、case1フォルダにjupyterでlesson1.ipynbファイルを作成します。  
次に、「【練習問題】お弁当の需要予測」のデータ、train.csv、test.csv、sample.csvをcase1フォルダに用意します。  
これらのファイルを使ってデータを分析します。

* Pandasについては[Pythonの勧め（3）Pandasについて](https://itstudio.co/2018/01/25/7502/)を参照ください。

csvファイル、tsvファイルをPandas.DataFrameとして読み込むには、Pandasの関数read_csv()を使う。

PandasのDataFrameは便利な関数やプロパティを使用することができます。

* header関数を使うと引数なしで上位5件のデータを表示します。また、引数に数値を入れると上位引数件のデータを表示します。
**行番号は0から始まります。**

* tail関数を使うと下位5件のデータを表示し、引数指定するとその件数だけ下位から順番に表示します。

* shapeプロパティは行数と列数を表示します。

今回はまずPandasのread_csv関数で、ダウンロードしてきた「【練習問題】お弁当の需要予測」のデータ、train.csv、test.csv、sample.csvをDataFrameとして読み込みます。

次に、header関数やtail関数を使用してデータを表示させます。

import文の記述は次の通りです。

```
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
%matplotlib inline
```

read_csv関数は次のように記述します。

```
train = pd.read_csv("train.csv")
```

header()は次のように記述します。

```
train.head()
```

詳細はコードを確認してください。

## Lesson2 Pamdasを使ってさらに詳細なデータ表示

基本統計量の表示を行うdescribe関数

describe() メソッドをでは、件数 (count)、平均値 (mean)、標準偏差 (std)、最小値(min)、第一四分位数 (25%)、中央値 (50%)、第三四分位数 (75%)、最大値 (max) を確認することができます。

```
train = pd.read_csv("train.csv")
train.describe()
```

* データの型はinfo関数で確認できます。

あるカラムだけ表示するには辞書型の表示をさせます。
```
train = pd.read_csv("train.csv")
train["y"]
````

特定のカラムの平均値、中央値、また条件設定もできます。
```
train["y"].mean() #平均値
train["y"].median() #中央値
train[train["y"] >= 150] #150以上のデータを表示
train[train["week"]=="月"] #曜日が月となっているデータのみ表示
```

カラムを2つ指定する場合は次のように記述
例はyとweekの2つのカラムを指定

```
train[["y","week"]]
```

