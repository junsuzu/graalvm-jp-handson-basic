# Oracle GraalVM Enterprise ハンズオン演習 (Basic編)

## ＜目的と対象＞：
このハンズオン演習は、次世代Polyglot(多言語対応）ランタイム環境であるOracle GraalVM Enterprise版の導入、操作手順を演習形式で学ぶためのガイドです。この演習を通じて以下の項目をマスターすることを目的としています。  
* GraalVMをPC環境への導入
* 新しいJITコンパイラー上でJavaクラスの実行
* 新しいAOTコンパイラー上でNative Imageの生成と実行
* Polyglot（多言語）プログラミングと実行  

このハンズオン演習の対象はJava基礎知識を有することは望ましいが、必須ではありません。  

※この内容はOracle主催の「Oracle GraalVM Enterprise Virtual Hands-On Lab」の演習部分にあたります。参加者はこちらの内容に沿って事前環境セットアップおよび当日演習を実施して頂けます。また単独でGraalVMの入門演習としてもご利用頂けます。  
<br/>

## ＜前提環境／事前準備＞
* ハンズオンの内容はWindows10でWSL(Windows Subsystem for Linux)を利用し、LinuxディストリビューションのUbuntu20.04をインストールした環境を前提に進みます。  
* Ubuntu以外のLinuxおよびMacOS、Windowsもサポートされます。
* Githubリポジトリーからダウンロードすることがあるので、インターネットに繋がる状態が必要です。  

※「Oracle GraalVM Enterprise Virtual Hands-On Lab」の参加者は基本的に事前セットアップ済みの環境でハンズオン演習を実施して頂きます。ただし、演習が不要な方は、演習部分を視聴のみして頂くことも可能です。  
<br/>


## ＜演習内容＞

* **[演習 1: GraalVM Enterpriseのインストール](#演習-1-GraalVM-Enterprise-Editionのインストール)**
   * [1.1: GraalVM EE20.1.0のダウンロード](#11-GraalVM-EE2010のダウンロード)
   * [1.2: GraalVM Coreパッケージのインストール](#12-GraalVM-Coreパッケージのインストール)
   * [1.3: Native Imageのインストール](#13-Native-Imageのインストール)
   * [1.4: LLVMとR言語プラグインのインストール](#14-LLVMとR言語プラグインのインストール)

* **[演習 2: High-performance JIT コンパイラー(coming soon)](#演習-2-High-performance-JIT-コンパイラーcoming-soon)**
* **[演習 3: Native Imageの生成と実行(coming soon)](#演習-3-Native-Imageの生成と実行coming-soon)**
* **[演習 4: Polyglotプログラミングと実行(coming soon)](#演習-4-Polyglotプログラミングと実行coming-soon)**
<br/>
<br/>


# 演習 1: GraalVM Enterprise Editionのインストール

以下はGraalVM Enterprise Edition 20.1.0 for JDK 8をインストール手順となります。

# 1.1: GraalVM EE20.1.0のダウンロード
(1) [OTN - Oracle Technology Network](https://www.oracle.com/downloads/graalvm-downloads.html)　からGraalVM Enterprise Editionをダウンロードします。下図のように"GraalVM Enterprise Edition 20 Current Release" タブを選択します。

  ![Download Picture 1](images/GraalVMinstall01.JPG) 

<br/>

(2)“Release Version 20.1.0, Java Version 8, Linux”を選択します。 MacOSを使用している場合macOSを選択してください。現時点でWindows版ではLinuxとMacOSに比べてGraalVMのフィーチャーが少ないので、この演習ではWindows版を選択しません。

  ![Download Picture 2](images/GraalVMinstall02.JPG)

<br/>

(3)OSを選択後、以下のコンポーネントをダウンロードします。必須コンポーネントはこの演習に必要です。その他のオプショナルコンポーネントは演習では使いませんが、今後GraalVMの多くの機能を試したい場合はダウンロードしておいてください。
*	Oracle GraalVM Enterprise Edition Core（必須）
*	Oracle GraalVM Enterprise Edition Native Image（必須）
* Ideal Graph Visualizer（オプショナル）
*	GraalVM LLVM Toolchain Plugin（必須）
*	Oracle GraalVM Enterprise Edition Ruby Language Plugin（オプショナル）
* GraalVM Enterprise Edition Python Language Plugin（オプショナル）
*	Oracle GraalVM Enterprise Edition WebAssembly（オプショナル）
* GraalVM R Language Plugin (必須）   
※R Pluginは直接ダウンロードできず、GraalVMのインストール・ユーティリティー(guコマンド)を使用してインストールします。)

  ![Download Picture 3](images/GraalVMinstall03.JPG)

上記コンポーネントをダウンロードするためには、OTNにログインする必要があります。OTNアカウントをお持ちでない方は以下のログイン画面から作成してください。


  ![Download Picture 4](images/GraalVMinstall04.JPG)

<br/>

コンポーネントをダウンロードした後、Linuxファイルシステム上に下記のようなモジュール一覧が表示されます：
* graalvm-ee-java8-linux-amd64-20.1.0.tar.gz(必須)
* idealgraphvisualizer-20.1.0-all.zip
* llvm-toolchain-installable-java8-linux-amd64-20.1.0.jar（必須）
*  native-image-installable-svm-svmee-java8-linux-amd64-20.1.0.jar(必須)
*  python-installable-svm-svmee-java8-linux-amd64-20.1.0.jar
*  namedruby-installable-svm-svmee-java8-linux-amd64-20.1.0.jar
* wasm-installable-svm-svmee-java8-linux-amd64-20.1.0.jar

 ![Download Picture 5](images/GraalVMinstall05.JPG)

<br/>

# 1.2: GraalVM Coreパッケージのインストール

(1)各モジュールをダウンロードした後、ダウンロード先のディレクトリーにて以下のコマンドを実行し、Coreパッケージを解凍します。
  >```sh
  >tar -zxf graalvm-ee-java8-linux-amd64-20.1.0.tar.gz
  >```
<br/>
上記コマンドの実行により”graalvm-ee-java8-20.1.0”というフォルダーが作成されます。
    
![Download Picture 6](images/GraalVMinstall06.JPG)

<br/>
フォルダーを任意のディレクトリーに移動します。そのディレクトリー配下がGraalVMのインストール先(Java Home)となります。下記の例ではフォルダーを/optの配下に移動する例です：

  >```sh
  >sudo mv graalvm-ee-java8-20.1.0 /opt/.
  >```

これにより、GraalVMのインストール・ディレクトリー（Java Home)は/opt/graalvm-ee-java8-20.1.0になります。

<br/>

(2)インストールしたGraalVMのパスを通すためには、以下のコマンドを実行します。

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

以上でGraalVM Coreパッケージのインストールが完了しました。確認するため以下のjavaコマンドを実行します。
  >```sh
  >java –version
  >```

<br/>
以下の出力結果を確認できれば、GraalVM 20.1.0 for Java8が正常にインストールされたことになります。

  >```sh
  >Java(TM) SE Runtime Environment (build 1.8.0_251-b08)
  >Java HotSpot(TM) 64-Bit Server VM GraalVM EE 20.1.0 (build 25.251-b08-jvmci-20.1-b02, mixed mode)
  >```
<br/>

# 1.3: Native Imageのインストール

(1)　Native ImageをインストールするのにGraalVM Utility(gu)を使用します。モジュールのダウンロード先にて下記コマンドを実行します。
  >```sh
  >gu install -L native-image-installable-svm-svmee-java8-darwin-amd64-20.1.0.jar
  >```

(2) Native Image依存ライブラリーのインストール

Native Imageの生成と実行は、以下３つのライブラリーが必要です。  
* glibc-devel: 標準 C ライブラリを使用した開発に必要なファイル
* zlib-devel: zip や gzip に使われている圧縮アルゴリズムをライブラリ化したもの
* gcc: C/C++など複数言語のコンパイラー集  

OSによりインストール・コマンドは異なります：  

Ubuntu
  >```sh
  >sudo apt-get install build-essential libz-dev zlib1g-dev
  >```

Oracle Linux
  >```sh
  >sudo yum install gcc glibc-devel zlib-devel
  >```
その他Linux
  >```sh
  >sudo dnf install gcc glibc-devel zlib-devel libstdc++-static
  >```
MacOS
  >```sh
  >xcode-select --install
  >
<br/>

# 1.4: LLVMとR言語プラグインのインストール

(1)LLVM-toolchainプラグインのインストール  
以下のコマンドを実行し、必要なモジュールを自動的にgithubよりダウンロードされます。
  >```sh
  >gu install llvm-toolchain
  >```
(2)R言語プラグインのインストール  
以下のコマンドを実行し、必要なモジュールを自動的にgithubよりダウンロードされます。
  >```sh
  >gu install R
  >```

(3)R言語ソースのコンフィグ  

以下のコマンドでR言語ソースのコンフィグ作業を実施します。
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

<br/>
インストール完了後、以下のguコマンドでインストールされたモジュールを確認します：

  >```sh
  >gu list
  >```

![Download Picture 7](images/GraalVMinstall07.JPG)


<br/>
<br/>
お疲れ様でした！以上でインストール作業はすべて完了しました。演習１では以下のタスクを実施しました。

1.	GraalVM EE20.1.0 for Java8のCoreパッケージのインストールおよびクラスパスの設定
2. Native Imageおよび依存ライブラリーのインストール
3. LLVMとR言語のインストール

<br/>
<br/>

# 演習 2: High-performance JIT コンパイラー(coming soon)
以下の演習は「Top 10 Things To Do With GraalVM」 の内容を使用します。  
https://medium.com/graalvm/graalvm-ten-things-12d9111f307d

(1)上記内容を使用するため、Githubよりソースをダウンロードします。以下のコマンドを実行します。

  >```sh
  >git clone https://github.com/marthenlt/native-image-workshop.git
  >```

(2)ダウンロードしたディレクトリーに移動します。

  >```sh
  >cd graalvm-ten-things
  >```

(3)以下のコマンドを実行し、large.txtファイルを作成します。この作業は時間がかかります。

  >```sh
  >make large.txt
  >```

(4)large.txtファイルが作成されたことをlsコマンドで確認します。サイズが150MBであることを確認してください。

![Download Picture 9](images/GraalVMinstall09.JPG)

(5)TopTen.javaはlarge.txtの中から単語を集計し、上位トップテンの単語一覧を出力するJavaプログラムです。このプログラムはStream Java APIを使用し、すべての単語をソート、カウントします。 

以下はプログラムの内容です。
```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.Arrays;
import java.util.function.Function;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class TopTen {

    public static void main(String[] args) {
        Arrays.stream(args)
                .flatMap(TopTen::fileLines)
                .flatMap(line -> Arrays.stream(line.split("\\b")))
                .map(word -> word.replaceAll("[^a-zA-Z]", ""))
                .filter(word -> word.length() > 0)
                .map(word -> word.toLowerCase())
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()))
                .entrySet().stream()
                .sorted((a, b) -> -a.getValue().compareTo(b.getValue()))
                .limit(10)
                .forEach(e -> System.out.format("%s = %d%n", e.getKey(), e.getValue()));
    }

    private static Stream<String> fileLines(String path) {
        try {
            return Files.lines(Paths.get(path));
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

}
```

(6)TopTen.javaをコンパイラーします。デフォルトではGraalVMのJavaコンパイラーが有効で、使用されます。

  >```sh
  >javac TopTen.java
  >```

(7)GraalVMのJITコンパイラーはJavaで書かれています。以下の最適化によりJITコンパイラーの実行速度が従来C++で書かれていたコンパイラーよりも速くなります。  
* Partial Escape Analysis  
* In-lining  
* Path Duplication

以下のJavaコマンドでコンパイルされたJavaクラスを実行し、実行タイムを測ります。引数にはlarge.txtを指定します。

  >```sh
  >time java TopTen large.txt
  >```

実行結果と実行時間を確認します。

```
sed = 502500
ut = 392500
in = 377500
et = 352500
id = 317500
eu = 317500
eget = 302500
vel = 300000
a = 287500
sit = 282500

real    0m32.699s
user    0m34.078s
sys     0m3.406s  
```  
(8)従来のJITコンパイラーとの比較のため、以下のJavaコマンドでフラッグを立てます：-XX:-UseJVMCICompile。JVMCIはGraalVMとJVMのあいだのインタフェースになります。

  >```sh
  >time java -XX:-UseJVMCICompiler TopTen large.txt
  >```

実行結果と実行時間を確認します。

```
sed = 502500
ut = 392500
in = 377500
et = 352500
id = 317500
eu = 317500
eget = 302500
vel = 300000
a = 287500
sit = 282500

real    0m48.901s
user    0m49.219s
sys     0m2.172s

```  
以上の結果により、GraalVMのJITコンパイラーによる実行時間は従来のHotSpotコンパイラーに比べて約３０％向上しました。  
<br/>
<br/>


# 演習 3: Native Imageの生成と実行(coming soon)

  
<br/>
<br/>

# 演習 4: Polyglotプログラミングと実行(coming soon)

