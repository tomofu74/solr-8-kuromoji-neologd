# solr-8-kuromoji-neologd
Apache Solr 8用のNEologd入りkuromojiをビルドするためのパッチです。

Apache Solr 8.1.1及び8.5.2で動作確認をしたものを置いてあります。

詳細については https://pandanote.info/?p=4637 をご参照ください。

## 以下は、参照者によるメモ

前提：　筆者の環境は, ubuntu 20.04 である。（原作者さんは fedora ）

1. mecab と apache ant がインストールされていることを確認する。

```
  $ sudo apt install mecab libmecab-dev mecab-ipadic-utf8
  $ sudo apt install ant
```


2. Apache solr のソースをダウンロードする。　e.g. solr-8.7.0-src.tgz
3. ダウンロードしたソースを展開し、展開したディレクトリでパッチを適用する。

```
  $ cd solr-8.7.0
  $ patch -p1 < solr-8.7.0-kuromoji-neologd.patch 
```

4. Neologd の github でNEologdの最新の更新日を確認。
5. 確認した更新日を、 lucene/build.propertiesのneologd.properties に指定。
6. 原作者のpandanote-infoさんに感謝の念を送りつつ、ソースのトップディレクトリで以下のコマンドを実行。

```
  $ ant ivy-bootstrap
  $ ant compile
  $ cd lucene/analysis/kuromoji
  $ ant clone-neologd
  $ ant build-dict
  $ ant jar-core
```
