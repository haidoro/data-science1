# 優良顧客を探せ

ミッション
* ある顧客が銀行口座を開設するかしないかを予測して、キャンペーンを効率化せよ。

予測問題
* 分類問題

評価尺度
* AUC(1がもっとも良い)

今回のモデル
* 決定木モデル

## 基礎分析

`train.shape`で行数と列数の確認ができる。

`train.describe()`で基礎統計量を確認できる。基礎分析を行うと言えば、この関数を使ってデータを確認すると良い。

項目名	意味
count	そのカラムの件数
mean	平均
std		標準偏差
min		最小値
25%		第一四分位数
50%		第二四分位数
75%		第三四分位数
max		最大値

trainの基礎分析結果とtestの基礎分析結果の差異があまりないか確認しておきます。あまりに違うと過学習になる可能性があります。

欠損値の有無

欠損値の有無は`isnull()`関数と`sum()`関数を併用します。

```
train.isnull().sum()
```

## 講座開設者の確認

講座開設者は「y」で表されます。その詳細を確認する場合は次のようにします。

```
train["y"].value_counts()
```

### marital(未婚/既婚)とyのクロス集計

* pd.crosstab関数を使います
* pd.crosstab(X["A"], X["B"])と書いた場合、Aが縦列、Bが横列となります
* 更にオプションとしてmargins=Trueを書くと、総計値のカラムまで作成されるので便利です
* 結果の「divorced」は離婚、 「married」は既婚、「single」は独身のことです。
```
pd.crosstab(train["marital"],train["y"],margins=True)
```

### ageをビニング

数値データを使ってクロス集計する場合はビニングという処理を行う必要があります。例えば年齢を使う場合などです。

* ageは数値データなので、クロス集計をする為にはビニングが必要
* ビニングとは数値データを例えば、グループ①（0より大きい、10以下）、グループ②（10より大きい、20以下）…のように集約すること
* ビニングはpd.cut関数を使います
* オプションには、①ビニングしたいデータ、②どう区切るのか？（例えば[0,10,20]）を書きます

まずtrain["age"]の基本統計量を確認します。

```
train["age"].describe()
```

その後、ビニングした結果を変数`age_bining`に代入しましょう。

```
age_bining = pd.cut(train["age"],[0,20,30,40,50,60,100])
```

age_biningとyを使ってクロス集計を行います。

```
pd.crosstab(age_bining,train["y"],margins=True)
```

## 優良顧客を探せ

ミッション
* ある顧客が銀行口座を解説するかしないかを予測してキャンペーンを効率化せよ
* 予測問題
　* 分類問題
* 評価尺度
  * AUC
* 今回のモデル
  * 決定木モデル
  
### 決定木とは
質問に対する分岐（True,False）を段階的に作ることで、判別や回帰を行うモデルのこと。

Graphvizが必要

Gtaphvizのインストール

Home Brewでインストールします。

```
brew install graphviz
```

### Anacondaへのモジュール追加

Anaconda Navigatorから追加できます。

今回はそのまま「base(root)」に追加します。  
左の「Environments」を選択して「installed」のプルダウンメニューを「Not Installed」に変更して「Search Packages」に「graphviz」を入力して「python-graphviz」が出てきたらインストールします。
次に、同様に「Search Packages」に「pydotplus」を入力して「pydotplus」が出てきたらインストールします。


今回のimportは複数のモジュールを読み込む必要があります。

```
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
%matplotlib inline
from sklearn.tree import DecisionTreeClassifier as DT
from sklearn.tree import export_graphviz
import pydotplus
from IPython.display import Image
```

### 説明変数

ilocを使って説明変数を取り出しています。

pandas.DataFrameの任意の位置のデータを取り出したり変更（代入）したりする場合、pandas.DataFrameのプロパティのat, iat, loc, ilocを使うことができます。

train.iloc[ 開始行:終了行, 開始列:終了列 ]と書くことで、列・行を数字で指定して書くことができます。

### 決定木モデル
決定木モデル作成はDT()を使います。  
DTのカッコの中にはオプションとして機械学習のパラメータを書きます。今回はmax_depth=2, min_samples_leaf=500と書きます。

決定木モデルを作成はfit関数を使います。  
カッコの中に、説明変数、目的変数の順番に書きます。

決定木を可視化する為には、まず決定木の図データをdotファイルで書き出し、その後、jupyter上で表示する必要があります。  
まずdotファイル書き出しには`export_graphviz`関数を使います。
`export_graphviz(clf1, out_file="tree.dot", feature_names=trainX.columns, class_names=["0","1"], filled=True, rounded=True)`と書きます。

書き出せたら、下記の２行を書きます。
`g = pydotplus.graph_from_dot_file(path="tree.dot")`  
`Image(g.create_png())`



