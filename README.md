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

## Lesson3グラフの描画

グラフの描画はMatplotlibを使います。  
[Matplotlibでグラフを描画](https://itstudio.co/2018/01/22/7432/)を参考にしてください。

グラフの描画はまずどの値を使うか決めます。  
今回はy（販売数量）の値をグラフ化します。

カラムの指定は前回までに練習した辞書形式で呼び出します。
あとはplot関数を指定するだけです。

```
train["y"].plot()
```
サイズを指定する場合は引数に縦横比を次の例のように記述します。

```
train["y"].plot(figsize=(12,4))
```
グラフにタイトルをつける場合も引数で指定

```
train["y"].plot(figsize=(12,4),title="販売数量推移")
```
タイトルが文字化けします。  
matplotlibは日本語に対応していません。

### matplotlibは日本語化対処方法

日本語フォントが入ってないため文字化けしています。
フリーフォントを見つけてきて下記の通り実行してみます。

* macの場合は`/Users/YOUR_NAME/anaconda3/lib/python3.6/site-packages/matplotlib/mpl-data/fonts/ttf`の中にコピーします。
  
ただし、Homebrewでインストールした場合は、`/Users/YOUR_NAME/.pyenv/versions/anaconda3-5.2.0/lib/python3.6/site-packages/matplotlib/mpl-data/fonts`となります。anaconda3-5.2.0などのバージョン番号は自分の環境に合わせてください。

* windowsの場合はC:\Anaconda\Lib\site-packages\matplotlib\mpl-data\fonts\ttfの中にコピー

matplotlibrcというファイル`/Users/tahara/.pyenv/versions/anaconda3-5.2.0/lib/python3.6/site-packages/matplotlib/mpl-data`の中身を下記のように変更します。

1. エディタでmatplotlibrcファイルを開く
2. #font.familyで検索をかける
3. #font.family : sans-serifという行がヒットするので、その行をコピー
4. 上記の行の下の行にコピーしたものを貼り付ける
5. 貼り付け後、貼り付けた行を次のように書きなおす。`#font.family : sans-serif　⇒　font.family : フォント名`
6. 保存

うまくいかない場合は英語表示でいきましょう。

## ヒストグラム描画

ヒストグラムの描画は次の記述をします。

```
train["y"].plot.hist()
```

グリッド線が必要な場合は以下

```
train["y"].plot.hist(grid=True)
```

画像として書き出すには次のようにします。

```
train["y"].plot.hist(figsize=(12,4),title="ヒストグラム")
plt.savefig("sample1.png")
```

## 箱ひげ図

箱ひげ図はデータ分布を確認するための図
中央値、上側ヒンジ、下側ヒンジ、並びに最大値、最小値を表すものです。

箱ひげ図を描くときは2つの値を指定します。

今回は販売数（y）と　weekを指定します。  
その指定方法は`train[["y","week"]]`のコードです。  
x軸に指定したいカラムは`boxplot(by="week")`とします。

```
train[["y","week"]].boxplot(by="week")
```

これで箱ひげ図が描けます。

## 欠損値

欠損値とはなんらかの理由によりデータの値が入ってないことを言います。
欠損値が有るか無いかは`isnull()`関数で確認できます。
Trueと表示されると欠損値が存在します。

```
train.isnull()
```

もう少し見やすくしたもので、カラムごとに欠損値があるかどうか調べるには次のように記述します。

```
train.isnull().any()
```

さらにそのカラムにどれくらいの数量の欠損値があったか調べるには次のようにします。

```
train.isnull().sum()
```

欠損値をなんらかの値で補完する場合`fillna`関数を使います。引数に指定した値が補完されます。

0で補完する例
```
train.fillna(0)
```

欠損値があるとそれを削除するには`dropna()`関数を使います。
けれども一つでも欠損値が存在するとその項目は全て削除されるためこの関数は使いづらいです。

```
train.dropna()
```

ある列に欠損値があった場合のみ、その行を削除したい場合はオプションとしてsubset=[ ]を使います。

```
train.dropna(subset=["kcal"])
```

ある項目に入っている値の数を表示する関数が`value_counts()`です。

```
train["precipitation"].value_counts()
```








