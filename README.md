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

## Lesson4 欠損値

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

## Lesson5 相関関係

- 相関関係とは、Aという事象とBという事象の間、双方向の動きに関係があることを言います
- 例えば、気温が上がると弁当の売り上げ数もあがる関係があった場合、正の相関があると言ったりします
- 逆に、気温が上がると弁当の売り上げが下がる関係があった場合、負の相関があると言います
- なお、相関関係と因果関係は異なる為、注意が必要です
- この関係の度合は相関係数と呼ばれる数値で表されます

相関係数を求めるには`corr()`関数を使います。

y(販売数量)とtempaerture(気温)の相関係数を求めるには次のようにします。

```
train[["y","temperature"]].corr()
```
## 散布図
相関係数は散布図を描くとわかりやすくなります。

散布図は`plot.scatter()`関数を使います。

```
train.plot.scatter(x="temperature",y="y",figsize=(5,5))
```

## Lesson6 予測モデル作成

### ポイント
* 予測を行うには基礎分析をまず行う。
* 学習のしすぎによって判断の基準が厳しくなるため、少しでもパターンが異なると誤った答えを出力してしまう場合がある。このことを過学習という。

### 目的変数と説明変数
目的変数（出力変数）：予測したい商品の売り上げ実績
説明変数（入力変数）：予測のヒントになりそうなもの　天気、気温、来店客数など。単回帰分析は一つだけ説明変数をとる。

### 代表的な予測問題
1. 回帰問題...目的変数が数値
2. 分類問題...目的変数がカテゴリ

回帰分析を行うには、学習と推論のフェーズがある。

* 汎用的な予測モデルを作成するには、元になるデータセットがあるとこれを2分割し、片方を学習モデル、もう一方を評価モデルとする。

# お弁当大作戦

* ミッション
 * お弁当の売り上げを予測して、廃棄ロスや機会損失をカット
 * 予測モデル：回帰問題
 * 今回のモデル
  * 単回帰モデル　y = ax + b
  * 重回帰モデル

## 必要なPythonのライブラリ

今回は`sklearn`を使います。
`sklearn`（サイキット・ラーン）はPythonの代表的な機械学習のライブラリです。

ライブラリのimport  
```
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
%matplotlib inline
from sklearn.linear_model import LinearRegression as LR
```

### 説明変数と目的変数を決める

説明変数：temperature
目的変数：y

```
trainX = train["temperature"]
y = train["y"]
```

### 単回帰の場合必要な処理

説明変数のデータに`values.reshape(-1,1)`処理が必要です。

```
trainX = trainX.values.reshape(-1,1)
testX = testX.values.reshape(-1,1)
```

### 回帰モデルの変数を準備

```
model1 = LR()
```

### 単回帰モデル作成

fit関数を使い、第1引数に「説明変数」、第2引数に「目的変数」

```
model1.fit(trainX,y)
```

### モデルの傾きや切片を求める

傾きはcoef_を使います。

```
model1.coef_
```

切片はintercept_を使います。

```
model1.intercept_
```

### 予測

predict関数を使います。

```
pred = model1.predict(testX)
```

sample[1]に予測結果を代入

```
sample[1] = pred
sample.head()
```

### sampleファイル書き出し

書き出しは`to_csv関数`を使います。

```
sample.to_csv("submit1.csv",index=None,header=None)
```

## モデルの評価
予想値と実測値から誤差を求める。
モデルの評価は評価関数を用いる。モデルの予測制度を評価する数式でRMSE(Root Mean Square Error) がある。式は次の通りで平均二乗誤差とも言われる。ん、標準偏差のこと？データサイエンスでは度々使われる式ですから意味など熟知しておくと良いです。

![ds1](images/ds1.png)

評価関数はそのほかにもAUC,LogLoss,Accuracy,Precision,Recall,MAE,MAP@N,nDOGなどがある。

## Lesson7 10回帰モデル

2つ以上の説明変数を使ったもの。
y = ax1 + bx2 + cx3

天気などはダミー変数を使う。晴れ=>1 雨=>0 曇り=>0などのようにしてマトリックスを作る。

trainのweekの値がそれぞれいくつあるかは`value_counts()`で調べます。

```
train["week"].value_counts()
```

trainのweekをダミー変数化するには`pd.get_dummies`関数を使います。

```
pd.get_dummies(train["week"])
```

trainからweekとtemperatureを抜きだし、ダミー変数化したものを説明変数trainXに代入します。今回はweekとtemperatureの2つを用意します。書き方は次の通り。

```
trainX = pd.get_dummies(train[["week","temperature"]])
```
目的変数を用意

```
y = train["y"]
```

回帰モデル用の変数を用意します。

```
model = LR()
```

重回帰モデルの作成

```
model.fit(trainX,y)
```

### 切片と傾きを求める

傾きはで`model.coef_`求め、切片は`model.intercept_`で求めます。

### 予測結果

testからweekとtemperatureを抜きだし、ダミー変数化したものを変数testXに代入

```
testX = pd.get_dummies(test[["week","temperature"]])
```

modelとtestXを使って予測をして、予測結果を変数名predに代入

```
pred = model.predict(testX)
```

sample[1]にpredを代入して、"submit3.csv"というファイル名でファイル出力

```
sample[1] = pred
sample.to_csv("submit3.csv",index=None,header=None)
```

重回帰モデルにしたと言っても必ずしも良い結果になるとは限りません。

## Lesson8 特徴量（説明変数）
特徴量を設定することで予測精度を上げることができる
与えられたデータや外部データを加工し、予測の手がかりとなりそうな新たな特徴を作ること

* 基本統計量を作る：平均値を活用
* データを集約する：年齢層別にする
特徴量が多いと過学習になる
* 単変異解析
* モデルベース選択
* 反復選択

販売数のyの推移を見ると時間の推移と共に落ち込みが出ている。
そのため、特徴量として時間（年月）を指定して精度の変化をみる。

trainのdatetimeから年と月のデータを取り出し、trainの新たなカラムとして追加してこれを特徴量とする。

datetimeの値から年と月を抜き出すにはPythonの`sprit()`関数を使う。
`apply()`関数はデータを変換するものです。`lambda`は無名関数ということです。

`lambda x :x.split("-")[0]`をJavaScript的に書くと`function(x){x.split("-")[0]}`となります。

この際、取り出した値はobject型のため、int型に変更します。

```
train["year"] = train["year"].astype(np.int)
train["month"] = train["month"].astype(np.int)
```

train,testからyearとmonthを取り出し、変数trainX,testXに代入

```
trainX = train[["year","month"]]
testX = test[["year","month"]]
```

重回帰モデル作成

```
model1 = LR()

model1.fit(trainX,y)
```

切片と傾きの確認

`model1.coef_`で切片、`model1.intercept_`で傾きが得られる。

testXを使って予測をし、予測結果を変数predに代入

```
pred = model1.predict(testX)
```

## 特徴量の作成
* trainXに対して予測を行えば、実際の売り上げyがわかっているので、その差を比較することができます
* そこでtrainXに対する予測値と、実際の売り上げyとを引き算することで、どの日が大きく予測を外していたかを確認します
* 大きく外れていたものから共通する要素が見つけられれば、その要素を加えることで更に精度が良いモデルを作れる可能性があります
* その準備として、まずmodel1とtrainXを使って、trainXに対する予測値を求めましょう

今回は「お楽しみメニュー」を特徴量に設定





