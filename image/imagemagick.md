## Imagemagick

CLIで画像変換できるコマンド。
サンプルコマンドを見れば使い方がだいたいわかるので載せておく。

pngからjpegにする。
```
convert a.png a2.jpeg
```
100以上のフォーマットに対応しているので、有名なフォーマットならOK。

サイズを150%にする。
```
convert a.png -resize '150%' a2.png
```

アスペクト比を無視して200x300のサイズに変更する（width=200, height=300）
```
convert a.png -resize '200x300!' a2.png
```

アスペクト比を保ってwidth=200に設定する
```
convert a.png -resize '200x' a2.png
```
heightの場合は'x200'のようにすればOK。

png8にする。
```
convert a.png png8:a2.png
```
