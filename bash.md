## bash
 Linuxでサポートされるシェルの一つ

## シェルの種類
- sh  
  Bourne Shellとも呼ばれる。
- bash  
  Bourne-Again Shell。shを大幅に強化。
- zsh  
  高機能なシェル。bash, tcsh, kshなどの機能を多く取り込んでいる。

## プロンプト
プロンプトは「うながす」の意味で、コマンドの入力を促すために表示される。  
末尾は一般ユーザーの場合は$, スーパーユーザーの場合は#となる。  

## シェル変数と環境変数
bashにはシェル変数と環境変数があります。シェル変数はそのシェルの中で使用できる変数、  
環境変数は子プロセスにも引き継がれる変数です。環境変数として定義された値はシェル変数としても参照できる。  

```
set             # シェル変数を一覧表示する
FOO=xxx         # シェル変数を設定する
echo $FOO       # シェル変数を参照する
unset FOO       # シェル変数をクリアする

env             # 環境変数を一覧表示する
export BAR=xxx  # 環境変数を設定する
echo $BAR       # 環境変数を参照する
unset BAR       # 環境変数をクリアする
```

主な環境変数
```
PATH  コマンドの検索パス(例:/usr/local/sbin:/usr/local/bin...)
HOME  ホームディレクトリ(例:/home/noda)
LANG  言語情報(例:en_US.UTF-8)
PWD   カレントディレクトリ(例:/home/noda/tmp)
_     前回実行したコマンドの最後の引数
```

## コマンド
- 標準出力
```
echo "Message" 
echo -n "Input your name: " (改行なし、よくわからない)
```

- 標準出力2  
%dや%sなどのフォーマットを用いて書き出す
```
printf "name=%s/age=%d\n" "Yamada" 26
```
### 初期化ファイル
ログイン時には次のファイルが実行される。  
環境変数の設定や初期化などに使用される。  
```
/etc/profile    ログイン時に実行される処理(全ユーザ)
~/.bash_profile ログイン時に一度だけ実行される処理(自分のみ)
~/.bashrc       シェル起動時に毎回実行される処理(自分のみ)
```

ログアウト時には次のファイルが実行されます。
```
/etc/bash.bash_logout ログアウト時に実行される処理(全ユーザ)
~/.bash_logout        ログアウト時に実行される処理(自分のみ)
```

### 入出力・リダイレクト(>)
コマンドは標準入力(stdin)から読み込み、  
標準出力(stdout), 標準エラー出力(stderr)に書き出すことができる。  
```
command < file # fileの内容をcommandの標準入力に渡す
```

コマンドの標準出力、標準エラー出力をファイルに書き出すには次のようにする。  
1は標準出力、2は標準エラー出力、&は両方を意味する。1>は>と省力できる。  
'>を>>にするとファイルを上書きするのではなく、追記するようになる。  
```
command > file            # 標準出力をfileに書き出す
command 1> file           # 標準出力をfileに書き出す(>と等価)
command 2> file           # 標準エラー出力をfileに書き出す
command 1> file1 2> file2 # 標準出力をfile1に、標準エラー出力をfile2に書き出す
command &> file           # 標準出力と標準エラー出力をfileに書き出す
command >& file           # &>と同義
```

### 参考
https://www.tohoho-web.com/ex/shell.html