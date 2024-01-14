## 画像フォーマットについて

画像フォーマットについてまとめる。

### Bitmap images

#### PNG

Portable Network Graphics。https://www.w3.org/TR/2003/REC-PNG-20031110/
可逆圧縮のBitmap imageフォーマット。可逆圧縮の画像フォーマットとして一番安パイな印象。
PNGの中でも色数によりPNG8やPNG24と分類したりすることもある。

##### ファイル構造

必ずファイル頭の「89 50 4E 47 0D 0A 1A 0A」のファイルシグネチャから始まる。
その後はチャンクが並ぶ構造。
チャンクには種類があるが、以下の3種類が主。他にもいろいろある。
- IHDR: ヘッダ
- IDAT: データ
- IEND: 終端

IHDRが先頭にあり以下などを指定する。
- 画像サイズ（幅、高さ）
- ビット深度
  - カラーのビットサイズ
- カラータイプ
  - グレースケールとか8bit-color、24-bit color（true color）など
- 圧縮方式

##### 圧縮

IHDRで指定できるが、標準としては以下の構成のみ指定可能。
- アルゴリズム: Deflate
- 実装: zlib

指定した圧縮方式によって主にIDATが圧縮される。

#### JPEG

Joint Photographic Experts Group。
非可逆圧縮画像フォーマット。
DCT（discrete consine transform）して、量子化ビット数を落として、ハフマン符号で圧縮。

#### WebP

「ウェッピー」と読む。Googleが開発した圧縮画像フォーマット。
可逆/非可逆の圧縮方式があるらしい。
Google曰く、PNG/JPEGよりも優秀らしいが、JPEGに関しては変わらないという調査もあったり。

#### GIF

Graphics Interchange Format。
可逆圧縮の画像フォーマットで、アニメーションが使えるのが最大の特徴。ただし256色しか使えない。
アニメーションを使わないならPNG8でよさそう。アニメーションを使うにしてもWebPでよさそう。
現代ではGIFの上位互換が多いのかもしれない。
アニメーションは各フレームの画像データセクションをつなげているようなかんじっぽい。
圧縮はLZW。


#### BMP

Microsoft Windows Bitmap Imageを略してBMPらしい。非圧縮のフォーマット。
最近もうあまり使われてなさそう。PNGでいい。
