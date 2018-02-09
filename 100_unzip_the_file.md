# SECCON 2015 Unzip the file
[問題](https://github.com/SECCON/SECCON2015_online_CTF/tree/master/Crypto/100_Unzip%20the%20file)

unzipなるファイルが与えられる．これを解凍しろとのこと．
とりあえず`file`コマンドで種類を確認すると普通のzipファイルであることがわかる．
```bash
$ file unzip
  =>unzip: Zip archive data, at least v2.0 to extract
```
普通に解凍しようとするとパスワードを聞かれる．

```bash
$ unzip -l unzip
```
すると
- backnumber08.txt
- backnumber09.txt
- flag
という3つのファイルが入っていることがわかる．

backnumber08.txt backnumber09.txtはググるとSECCONの過去資料がダウンドーロできる．
backnunber08.txtの原文が手に入った．zipファイルにはこれをなんらかの方法で暗号化したバイナリファイルなので，うまい具合に比較すればzipの中身を復元できそう．
wikipediaの[暗号解読](https://ja.wikipedia.org/wiki/%E6%9A%97%E5%8F%B7%E8%A7%A3%E8%AA%AD)のページによると，「既知平文攻撃」というらしい．

「zip 既知平文攻撃」でググると，pkcrackを使ってzipのパスワードを解析する記事が出てきた．

それを参考にパスワードをクラックしてみる
```
zip myzip.zip backnumber08.txt backnumber09.txt
pkcrack -C unzip -c backnumber09.txt -p backnumber09.txt -P myzip.zip
#Ta-daaaaa! key0=XXX, key1=YYY, key2=ZZZ
zipdecrypt XXX YYY ZZZ unzip decrypt.zip
```

すると，パスワードのかかっていない`decrypt.zip`が手に入る．中身はflagという名のファイルで，実態はMS word．
開くと白い文字でflagが書いてある．