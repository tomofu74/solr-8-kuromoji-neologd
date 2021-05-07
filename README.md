# solr-8-kuromoji-neologd
Apache Solr 8用のNEologd入りkuromojiをビルドするためのパッチです。

Apache Solr 8.1.1及び8.5.2で動作確認をしたものを置いてあります。

詳細については https://pandanote.info/?p=4637 をご参照ください。

## 以下は、参照者によるメモ

前提：　参照者（筆者）の環境は, ubuntu 20.04 である。（原作者さんは fedora ）
　筆者は、 solr ver.8.7.0 で NEologd 辞書を使いたい・・・。

1. mecab と apache ant がインストールされていることを確認する。

```
  $ sudo apt install mecab libmecab-dev mecab-ipadic-utf8
  $ sudo apt install ant
```


2. Apache solr のソースをダウンロードする。　e.g. solr-8.7.0-src.tgz
3. ダウンロードしたソースを展開し、展開したディレクトリでパッチを適用する。

```
  $ git clone https://github.com/tomofu74/solr-8-kuromoji-neologd.git
  $ wget https://archive.apache.org/dist/lucene/solr/8.7.0/solr-8.7.0-src.tgz
　$ tar zxf solr-8.7.0-src.tgz
  $ cd solr-8.7.0
  $ patch -p1 < ../solr-8-kuromoji-neologd/solr-8.7.0-kuromoji-neologd.patch 
```

4. Neologd の github でNEologdの最新の更新日を確認。 20200910 が、このgit でのデフォルトバージョン。
5. 確認した更新日を、 lucene/build.propertiesのneologd.properties に指定。
6. 原作者のpandanote-infoさんに感謝の念を送りつつ、ソースのトップディレクトリで以下のコマンドを実行。
 clone-neologd は、githubからのファイルダウンロードの時間も含めて20分弱かかる。コンソールの表示が更新されないが、不安に負けずにガチホ。
 筆者の環境では、build-dict が　java error 137 で build failed。　PCを再起動して再度実行すると成功。　PCの熱暴走か？

```
  $ ant ivy-bootstrap
  $ ant compile
  $ cd lucene/analysis/kuromoji
  $ ant clone-neologd
  $ ant build-dict
  $ ant jar-core
```

7. lucene/build/analysis/kuromojiの下に以下のような名前のjarファイルが作成されます。

```
lucene-analyzers-kuromoji-ipadic-neologd-<lucene version>-SNAPSHOT-<creation date of neologd>.jar
```

7. インストールは、solr を一旦停止して、 7. の jarファイル を solr webアプリのディレクトリの　WEB-INF/lib の下にコピーして、original の kuromoji を排除。
8. solr をリスタート。