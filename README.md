# Rで作成した表をワードファイルに出力

卒業研究などで表を作成する際、
エクセルを使用する人が多いと思います。
ここでは、Rで解析した結果を表にまとめる際に、
直接文書ファイルに出力する方法を共有しようと思います。

## 表の作成

はじめに、出力する表をRで作成します。
使用するパッケージを読み込みます。

```
library(tidyverse)
library(gt)
```

