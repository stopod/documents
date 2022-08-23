## リーダブルコード

読んだメモっぽいもの  
タイトル: The Art of Readable Code  
著者: Dustin Boswell  
訳: 角 征 典  

" The Art of Readable Code by Dustin Boswell and Trevor Foucher. Copyright 2012 Dustin Boswell and Trevor Foucher, 978-0-596-80229-5"  

Dustin Boswell. リーダブルコード (Kindle の位置No.99-100). Kindle 版. 

---

### 1章 理解しやすいコード

- コードは理解しやすくなければならない
- コードは他の人が最短時間で理解できるように書かなければならない
---

### 2章 名前に情報を詰め込む

- 明確な単語を選ぶ
  - 「get」はあまり明確な単語ではない
    - 「GetPage()」よりは「FetchPage()」「DounloadPage()」の方が明確

- 汎用的な名前を避ける（あるいは使う状況を選ぶ）
  - retvalには戻り値です以外の情報はない（戻り値なのは自明）
  - 良い名前というのは、「変数の目的や値を表すもの」
  - 汎用的な名前は情報の一時的な保管に限っては問題がない（生存期間が数行）  
  　明確な理由がある場合は別である
  - ループイテレータ（i, j, k, iter...）
    - club_i, members_i, users_iとかci, mi, uiにすると良いらしい（本当か？）

  少しでも時間を使って良い名前を考える習慣をつけるようにすれば、  
  すぐに「命名力」の高まりが感じられるようになるだろう  
  （命名力の高まりってのがおもしろい）

- 抽象的な名前よりも具体的な名前を使う
  - 「ServerCanStart()」よりも「CanListenOnPort()」のほうが具体的である

- 接尾辞や接頭辞を使って情報を追加する
  - 時価やバイト数のように計測できるものがあれば、変数名に単位を入れると良い
  - その他の重要な属性を追加する
    - plaintext, unescaped, utf8, urlenc...
      - すべての変数名に属性を追加しろということではなく、変数の意味を間違えてしまったときに  
        バグになりそうなところにだけ使うことが大切である
  - ミリ秒を表す変数名には、後ろに_msをつける、これからエスケープが必要な変数名には、前にraw_をつける

- 名前の長さを決める
  - 良い名前を選ぶときには、「長い名前を避ける」という暗黙的な制約がある  
  - スコープが小さければ短い名前でもいい
  - 長い名前を入力するのは問題じゃない
    - プログラミングに使うテキストエディタいは「単語補完」機能がついている
  - プロジェクト固有の省略形はダメ
    - docがdocumentで、strがstringであることはプログラマなら分かる
    - 不要な単語を投げ捨てる
      - 「ConvertToString()」を「ToString()」にしても必要な情報は何も損なわれていない

- 名前のフォーマットで情報を伝える
  - 例えばクラスのメンバ変数にアンダースコアをつけて、ローカル変数と区別する  
    （例えばの話）
---

### 3章 誤解されない名前

  - 名前が「他の意味と間違えられることはないだろうか？」と何度も自問自答する
    - 「filter()」  -> 選択するのか除外するのかわからない  
      「選択する」のであればselect()で、「除外する」のであればexlude()にしたほうが良い
    - 「Clip(text, length))」  -> 最後からlength文字を削除か、最大length文字まで切り詰めるかが不明瞭
    - 限界値を含めるときはmin, maxを使う
    - 範囲を指定するときはfirstとlastを使う
    - 包括, 排他的範囲にはbegin, endを使う
---

### 4章 美しさ

  - 3つの原則
    - 読み手が慣れているパターンと一貫性のあるレイアウトを使う
    - 似ているコードは似ているように見せる
    - 関連するコードをまとめてブロックにする

  - メソッドを使って整列をする

  - 縦の線をまっすぐにする
    - 試しにやってみて手間になるようだったら、その時は止めればいい
  - 一貫性と意味のある並び
    - 対応するHTMLフォームのinputフィールドと同じ並び順にする
    - 最重要なものから重要度順に並べる
    - アルファベット順に並べる
      - どの並び順を選ぶにしても、一連のコードでは同じ並び順を使うべき
    - 宣言をブロックにまとめる
      ``` 
      class FrontendServer {
        public: 
          FrontendServer();
          void ViewProfile( HttpRequest* request);
          void OpenDatabase( string location, string user);
          void SaveProfile( HttpRequest* request);
          string ExtractQueryParam( HttpRequest* request, string param);
          void ReplyOK( HttpRequest* request, string html);
          void FindFriends( HttpRequest* request);
          void ReplyNotFound( HttpRequest* request, string error);
          void CloseDatabase( string location);
          ~ FrontendServer();
        };
      ```
        ▽
      ``` 
      class FrontendServer {
        public: 
          FrontendServer();
          ~ FrontendServer();

          // ハンドラ
          void ViewProfile( HttpRequest* request);
          void SaveProfile( HttpRequest* request);
          void FindFriends( HttpRequest* request);

          // リクエストとリプライのユーティリティ
          string ExtractQueryParam( HttpRequest* request, string param);
          void ReplyOK( HttpRequest* request, string html);
          void ReplyNotFound( HttpRequest* request, string error);

          // データベースのヘルパー
          void OpenDatabase( string location, string user);
          void CloseDatabase( string location);
        };
      ```

  - コードを「段落」に分割する  
    文章は複数の段落に分割されている
      - 似ている考えをグループにまとめて、他の考えと分けるため
      - 視覚的な「踏み石」を提供できるから（ページの中で自分の場所を見失わないように）
      - 段落単位で移動できるようになる  
      -> 同じ理由でコードも「段落」に分けるべきである  
      段落ごとに要約コメントを追加してざっと目を通せるようにする

  - 個人的な好みと一貫性  
    最終的には個人の好みになってしまうこともある  
    （例えばクラス定義の開き括弧の位置とか）  
    どちらを選んでもコードの読みやすさに大きな影響はない、  
    ただ2つのスタイルを混ぜてしまうと読みにくくなる  
    -> プロジェクトの規約に従う、一貫性のあるスタイルは「正しい」スタイルよりも大切である
---

### 5章 コメントすべきことを知る

  - コメントの目的は、書き手の意図を読み手に知らせることである
    - 「コードの動作を説明する」ことは目的のごく一部
  
  - コメントするべきでは「ない」こと
    - 価値のないコメント例 -> コードからすぐにわかることをコメントに書かない
      ```
      // Accountクラスの定義
      class Account {
        public: 
          // コンストラクタ
          Account();
          
          // profitに新しい値を設定する
          void SetProfit( double profit);
          
          // このAccountからprofitを返す
          double GetProfit();
      };
      ```
    - コードを見ればどのように動くかわかることでも、コメントを読んだほうが早く理解できることは別
  
  - コメントのためのコメントをしない  
    「宿題に出したコードの関数には必ずコメントをつけろ」なんていう教師がいるとする  
    そうすると、コメントをつけていない裸の関数に罪悪感を抱き、関数の名前と引数をそのまま文章形式でコメントに書き直すようになってしまう
    - 悪い例
      ```
      // 与えられ たsubtreeに含まれるnameとdepthに合致したNodeを見つける。
      Node* FindNodeInSubtree( Node* subtree, string name, int depth);
      ```
    
    - 改善例
      ```
      // 与えられた'name'に合致したNodeかNULLを返す。 
      // もしdepth<= 0ならば、'subtree'だけを調べる。
      // もしdepth == Nならば、'subtree'とその下のN階層まで調べる。
      Node* FindNodeInSubtree( Node* subtree, string name, int depth);
      ```
  
  - ひどい名前はコメントをつけずに名前を変える  
    コメントはひどい名前の埋め合わせに使うものではない
    - 悪い例  
      CleanReply()という関数につけたコメント  
      -> このコメントはcleanの意味を分かりやすく説明しているだけ  
        「制限を課す（enforce limit）」という言葉を関数名に入れたほうが良い
      ```
      // Replyに対してRequestで記述した制限を課す。
      // 例えば、返ってくる項目数や合計バイト数など。
      void CleanReply(Request request, Reply reply);
      ```
    
    - 改善例
      ```
      //'reply'を'request'にある項目数やバイト数の制限に合わせる。
      void EnforceLimitsFromRequest(Request request, Reply reply);
      ```
  
  - 「優れたコード > ひどいコード + 優れたコメント」

  - 自分の考えを記録する  
    優れたコメントというのは「考えを記録する」ためのものである

    - 「監督のコメンタリー」を入れる
        - // このデータだとハッシュテーブルよりもバイナリツリーほのうが40%速かった
        - // 左右の比較よりもハッシュの計算コストのほうが高いようだ
        - ヒューリスティックだと単語が漏れることがあるが仕方ない。100%は難しい
        - このクラスは汚くなってきている
        - サブクラスhogehogeを作って整理したほうが良いかもしれない
      
    - コードの欠陥にコメントをつける
      - // TODO: もっと高速なアルゴリズムを使う
      - // TODO: JPEG以外のフォーマットに対応する
      - プログラマがよく使う記法はいくつかある  ->  大切なのは、そのコードをどうしたいのかを自由にコメントを書くこと
        - TODO: あとで手を付ける
        - FIXME: 既知の不具合があるコード
        - HACK: あまりキレイじゃない解決策
        - XXX: 危険！大きな問題がある
    
    - 定数にコメントをつける（ここまで）
