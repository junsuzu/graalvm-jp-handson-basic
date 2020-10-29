# GraalVM ハンズオン　ワークショップ
_Wednesday, 9 December 2020_

内容:

* **[Exercise 1: 前提環境／事前準備](#exercise-1-前提環境事前準備)**
* **[Exercise 2: GraalVM Enterpriseのインストール](#Exercise-2-GraalVM-Enterprise-Editionのインストール)**
   * [2.1: GraalVM EE20.1.0のCoreパッケージ](#21-GraalVM-EE2010のCoreパッケージ)
   * [2.2: Native Image, LLVM-toolchain, R言語コンポーネント](#22-Native-Image-LLVM-toolchain-R言語コンポーネント)
   * [2.3: R言語のコンフィグ](#23-R言語のコンフィグ)
   * [2.4: Native Imageの依存ライブラリー](#24-Native-Imageの依存ライブラリー)



# Exercise 1: 前提環境／事前準備

本ワークショップを実施するための前提環境および事前準備作業を以下に記述します。Workshop当日は基本的に参加者に事前準備なされた環境のもとでハンズオン演習を実施して頂きます。

※ただし、ハンズオン演習部分を省きたい場合方は、GraalVMに関するプレゼンやDemoをご覧になりながら受講のみして頂くことも可能です。


  * 本ワークショップはLinux環境（WindowsマシーンのWSL環境を含む）を前提に進みます。（MacおよびWindows自体はGraalVMをサポートしますが、本ワークショップではUbuntu20.04の環境で実施、検証されたものです。
  * GitとCurlを事前にインストールしておいてください。
  * Githubリポジトリーからダウンロードすることがあるので、インターネットに繋がる状態が必要です。
  * 

# Exercise 2: GraalVM Enterprise Editionのインストール

以下はGraalVM Enterprise Edition 20.1.0 for JDK 8をインストール手順となります。

# 2.1: GraalVM EE20.1.0のCoreパッケージ
(1) [OTN - Oracle Technology Network](https://www.oracle.com/downloads/graalvm-downloads.html)　からGraalVM Enterprise Editionをダウンロードします。下図のように"GraalVM Enterprise Edition 20 Current Release" タブを選択します。

  ![Download Picture 1](images/GraalVMinstall01.JPG)

(2)“Release Version 20.1.0, Java Version 8, Linux”を選択します。 MacOSを使用している場合macOSを選択してください。現時点でWindows版ではLinuxとMacOSに比べてGraalVMのフィーチャーが少ないので、本ワークショップではWindows版を選択しません。

  ![Download Picture 2](images/GraalVMinstall02.JPG)

(3)OSを選択後、以下３つのコンポーネントをダウンロードしてください。
1.	Oracle GraalVM Enterprise Edition Core
2.	Oracle GraalVM Enterprise Edition Native Image
3.	GraalVM LLVM Toolchain Plugin
また、本ワークショップでは上記３つのコンポーネント以外に、”GraalVM R Language Plugin” も必要になりますので、後にGraalVMのインストール・ユーティリティー(gu)を使用して該当コンポーネントをインストールします。

  ![Download Picture 3](images/GraalVMinstall03.JPG)
  本ワークショップでは使いませんが、オプションとして以下のコンポーネントも併せてインストールしてみてください：
1.	GraalVM Enterprise Edition Python Language Plugin
2.	Oracle GraalVM Enterprise Edition Ruby Language Plugin
3.	Oracle GraalVM Enterprise Edition WebAssembly Language Plugin
上記バイナリーをダウンロードするために、OTNにログインする必要があります。OTNアカウントをお持ちでない方は以下のログイン画面から作成してください。


  ![Download Picture 4](images/GraalVMinstall04.JPG)

モジュールをダウンロード後Linuxのファイルシステム上、下記のようにファイル一覧になります（赤線で囲まれている部分は本ワークショップで使うモジュールです）：
* graalvm-ee-java8-linux-amd64-20.1.0.tar.gz
* llvm-toolchain-installable-java8-linux-amd64-20.1.0.jar
*  native-image-installable-svm-svmee-java8-linux-amd64-20.1.0.jar
*  python-installable-svm-svmee-java8-linux-amd64-20.1.0.jar
*  namedruby-installable-svm-svmee-java8-linux-amd64-20.1.0.jar
* wasm-installable-svm-svmee-java8-linux-amd64-20.1.0.jar

 ![Download Picture 5](images/GraalVMinstall05_1.JPG)


(4)モジュールをダウンロード後、以下のように解凍します
  >```sh
  >tar -zxf graalvm-ee-java8-linux-amd64-20.1.0.tar.gz
  >```

上記コマンドの実行により”graalvm-ee-java8-20.1.0”というフォルダーが作成されます。
    
![Download Picture 6](images/GraalVMinstall06.JPG)

そのフォルダーをJava Homeとなる任意のディレクトリーに移動してください。下記の例では/optの配下に移動する例です：
  >```sh
  >sudo mv graalvm-ee-java8-20.1.0 /opt/.
  >```

これにより、GraalVMのインストールディレクトリーは/opt/graalvm-ee-java8-20.1.0になります。

(5)インストールしたGraalVMのパスを通すためには、以下のコマンドを実行します。

bashの場合
  >```sh
  >vi ~/.bashrc
  >```
zshの場合
  >```sh
  >vi ~/.zshrc
  >```


以下の行を ~/.zshrc もしくは ~/.bashrc に追加します。
  >```sh
  >export GRAALVM_HOME=/opt/graalvm-ee-java8-20.1.0
  >export PATH=$GRAALVM_HOME/bin:$PATH
  >export JAVA_HOME=$GRAALVM_HOME
  >```

ファイルを修正後、以下のコマンドで実行します。

bashの場合
  >```sh
  >source ~/.bashrc
  >```
zshの場合
  >```sh
  >source ~/.zshrc
  >```

以上でGraalVMのインストールが完了します。確認するため以下のjavaコマンドを実行します。
  >```sh
  >java –version
  >```

以下の出力結果を確認できれば、GraalVM 20.1.0 Java8が正常にインストールされたことになります。
  >```sh
  >Java(TM) SE Runtime Environment (build 1.8.0_251-b08)
  >Java HotSpot(TM) 64-Bit Server VM GraalVM EE 20.1.0 (build 25.251-b08-jvmci-20.1-b02, mixed mode)
  >```

# 2.2: Native Image, LLVM-toolchain, R言語コンポーネント

(6)　最後の手順として、Native Image, LLVM toolchain, Rをインストールします。インストールするのにGraalVM Utility guを使用します。
  >```sh
  >gu install -L native-image-installable-svm-svmee-java8-darwin-amd64-20.1.0.jar
  >```

  >```sh
  >gu install -L llvm-toolchain-installable-java8-darwin-amd64-20.1.0.jar
  >```

  >```sh
  >gu install R
  >```
(※Rのインストール時、必要なモジュールを自動的にgithubよりダウンロードされます)]

インストール後、以下のguコマンドでインストールされたモジュールを確認します：
  >```sh
  >gu list
  >```

![Download Picture 7](images/GraalVMinstall07.JPG)


# 2.3: R言語のコンフィグ

インストール完了後、以下のR言語ソースのコンフィグ作業も実施してください。
  >```sh
  >/opt/graalvm-ee-java8-20.1.0/jre/languages/R/bin/configure_fastr
  >```

以下の出力結果を確認します。

 
    The basic configuration of FastR was successfull.

    Note: if you intend to install R packages you may need additional dependencies.
    The following packages should cover depenedencies of the most commonly used R packages:
    On Debian based systems: apt-get install build-essential gfortran libxml2 libc++-dev
    On Oracle Linux: yum groupinstall 'Development Tools' && yum install gcc-gfortran

    Default personal library directory (/home/mluther/R/x86_64-pc-linux-gnu-library/fastr-20.1.0-3.6) does exist. Do you wish to create it? (Yy/Nn) y
    Creating personal library directory: /home/mluther/R/x86_64-pc-linux-gnu-library/fastr-20.1.0-3.6
    DONE

# 2.4: Native Imageの依存ライブラリー

Native Imageの実行は、glibc-devel, zlib-devel, gccの三つのライブラリーが必要です。下記コマンドでインストールしてください。
  >```sh
  >sudo apt-get install build-essential libz-dev zlib1g-dev
  >```

おめでとうございます！以上の作業で本ワークショップに必要な環境設定がすべて完了しました。以下のタスクが実施されました：
1.	GraalVM EE20.1.0のCoreパッケージをインストールし、クラスパスを設定しました。
2.	Native Image, LLVM-toolchain, R言語の三つのコンポーネントをインストールしました。
3.	Rのコンフィグを実施しました。
4. Native Imageに必要なライブラリーをインストールしました

