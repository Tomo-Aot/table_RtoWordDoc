# Rで作成した表をワードファイルに出力

卒業研究などで表を作成する際、
エクセルを使用する人が多いと思います。
ここでは、Rで解析した結果を表にまとめる際に、
直接文書ファイルに出力する方法を共有しようと思います。
以前のバージョンでは gtsave()
で保存する際にいくつかの形式が使えましたが、
現在は文書ファイルに出力できないので、
別の方法を考えてみました。

## 表の作成の準備

はじめに、出力する表をRで作成します。
使用するパッケージを読み込みます。

```
library(tidyverse)
library(gt)
```

次に、表にまとめたい情報をデータフレームにまとめます。
データフレームにまとめる際は、$tidyverse$ パッケージの
tibble() 関数を使用します。
この関数を使うことで、列名の型が表示されるので便利です。

```
list = tibble(
  sp = c("A", "B", "C"),
  value = c(100, 25,  60)
)
```

他のファイルを読み込んでRで使用する際は、
読み込んだデータフレームにパイプ演算子で
as_tibble() 関数に渡します。

```
your_data |> as_tibble()
```

## 表の作成

次に、データフレームから表に必要なデータを準備します。
今回は、iris のデータセットを用いて種ごとの花弁の長さの平均値と標準偏差を表にまとめます。

```
df = iris |> as_tibble()

table = df |> 
  group_by(Species) |> 
  summarise(
    mean = mean(Petal.Length),
    sd = sd(Petal.Length)
  )
```

種ごとに花弁の長さの平均値と標準偏差をまとめました。
このデータを gt パッケージの gt() 関数を使って表にします。

```
table |> gt()
```

これを実行すると、viewer に表が出力されます。
gtsave() 関数を使って出力結果を保存することはできますが、
文書ファイルには対応していません。

## Rmarkdownを使用

gtsave() 関数は使うことができないので、
別の方法を考えます。
Rmarkdown ファイルはRで出力することができる文書ファイルです。
この形式は、文書を出力する際に R の出力結果を表示することができます。
そのため、この方法を使用して表を文書ファイルに出力します。

ファイルを出力するために設定します。
ワードファイルに出力するため、
output: は word_document
output-file: はWord Document
に設定します。
出力する際は Knit で実行することができます。

```
---
title: "Document title"
author: "your name"
date: "year-month-date"
output: word_document
output-file: Word Document
---
```

Rmarkdown はフォーマットを設定して
Render することもできますが、
ワードファイルの表を出力することができなかったため、
解決策を考えてみます。

設定が終わったら、表を出力するスクリプトを記述します。
ファイルの設定の下に表を出力します。

```{r}
library(tidyverse)
library(gt)
df = iris |> as_tibble()

table = df |> 
  group_by(Species) |> 
  summarise(
    mean = mean(Petal.Length),
    sd = sd(Petal.Length)
  )

table |> gt()
```

Knit が完了すると、フォルダ内に新しくワードファイルが生成されます。
ファイル名は Rmarkdown と同じですが、拡張子はdocxになります。
