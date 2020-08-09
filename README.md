# nablarch-document
OSS版Nablarchドキュメントです。

## 前提
本ドキュメントはSphinxでビルドすることを前提としています。

## ドキュメントビルドの環境構築
### ビルドにDockerを使用しない場合
Python 3.6.x 及び以下のプラグインををインストールし、ローカルPCでドキュメントをビルドします。
* Sphinx(1.6.3)
* javasphinx(0.9.15)
* sphinx-rtd-theme(0.2.4)

### ビルドにDockerを使用する場合
Dockerイメージをビルドし、そのイメージを使用してドキュメントをビルドします。

#### 環境
- WSL2 + Docker Desktop for Windows
- プロキシがない環境を想定しています。  
  プロキシが存在する場合は適宜Dockerファイルを修正してください。

#### Dockerイメージのビルド方法
```
docker build -t nablarch-document-build .
```


## ドキュメントのビルド方法
### ビルドにDockerを使用しない場合
**日本語**
```bash
make html ja
```

**英語**
```bash
make html en
```

### ビルドにDockerを使用する場合
**日本語**
```bash
docker run -v <リポジトリをクローンしたディレクトリ(フルパス)>:/root/document nablarch-document-build /bin/bash -c "cd /root/document; sphinx-build -d _build/.doctrees -b html ja _build/html"
```

**英語**
```bash
docker run -v <リポジトリをクローンしたディレクトリ(フルパス)>:/root/document nablarch-document-build /bin/bash -c "cd /root/document; sphinx-build -d _build/.doctrees/en -b html en _build/html/en"
```


## textlintの実行方法
### 環境構築
#### Dockerを使用しない場合
##### Node.js

Node.jsをインストールします。
（v8.9.3で動作確認済み）

##### npm install

npmで依存ライブラリをインストールします。

```sh
npm install
```

##### docutils-ast-writer

[textlint-plugin-rst](https://github.com/jimo1001/textlint-plugin-rst)の依存ライブラリである
docutils-ast-writerをインストールします。

```sh
pip install docutils-ast-writer
```

#### Dockerを使用する場合
ドキュメントビルドの環境構築と同様です。

### 疎通確認
#### Dockerを使用しない場合

```sh
./node_modules/.bin/textlint .textlint/test/test.rst
```

`./node_modules/.bin`をPATHに設定しておくと、以下のように実行できる。

```sh
textlint .textlint/test/test.rst
```

#### Dockerを使用する場合

```sh
docker run -v <リポジトリをクローンしたディレクトリ(フルパス)>:/root/document nablarch-document-build /bin/bash -c "cd /root/document; ../node_modules/.bin/textlint .textlint/test/test.rst"
```


### 実行
#### Dockerを使用しない場合

対象ディレクトリを指定してtextlintを起動します。

```sh
./node_modules/.bin/textlint ja/development_tools
```

#### Dockerを使用する場合

対象ディレクトリを指定してtextlintを起動します。

```sh
docker run -v <リポジトリをクローンしたディレクトリ(フルパス)>:/root/document nablarch-document-build /bin/bash -c "cd /root/document; ../node_modules/.bin/textlint ja/development_tools"
```


### 設定ファイル

| ファイル               | 説明           |
|------------------------|----------------|
| .textlintrc            | textlintの設定 |
| .textlint/conf/prh.yml | 辞書的なやつ   |

