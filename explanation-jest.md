---
marp: true
---

# jest を理解する

---

# jest とは何か

https://jestjs.io/ja/

> Jest はシンプルさを重視した、快適な JavaScript テスティングフレームワークです。

---

- 高速で安全

  > テストが一意なグローバル状態を持つことを保証することで、Jest は安全にテストを並列実行できます。開発効率が上がるよう、Jest は以前に失敗したテストを最初に実行し、テストファイルの所要時間に基づいて再整理します。

- コードカバレッジ

  > フラグ --coverage を指定することで、コードカバレッジを生成します。Jest は未テストのファイルを含むプロジェクト全体からコードカバレッジ情報を収集できます。

- JEST の哲学
  > 親しみやすく、豊富な機能を持つ API によって簡単にテストを書くことができ、さらには素早く結果を得ることができます。

---

# 基本の構成

```
test('two plus two is four', () => {
  expect(2 + 2).toBe(4);
});
```

- test から始まる、it でもいける
- `toBe`が Matcher と呼ばれるもの
- `expect`に比較したいものを、それを Matcher を利用して確認していく感じ
  <br>
  あとは以下の公式を参考に
  https://jestjs.io/ja/docs/using-matchers

---

# 非同期コードのテストについて

時間が余れば読もうかな、いったんパス  
https://jestjs.io/ja/docs/asynchronous

---

# セットアップと破棄

以下の 4 つを覚えとくと都合いい

- `beforeAll(() => {})`
  すべてのテストの前に 1 度だけ実行される
- `afterAll(() => {})`
  すべてのテストの後に 1 度だけ実行される
- `beforeEach(() => {})`
  各テストの前に実行される
- `afterEach(() => {})`
  各テストの後に実行される

---

# スコープ

> すべてのテストの前に..

と書いたけど、実はそのまま認識すると誤りがある。  
これはスコープという概念があるので。
そのスコープに存在するテストすべて、という意味として認識すること。
スコープとはなんぞやというと`describe`← これ
例を公式から読んでみる  
https://jestjs.io/ja/docs/setup-teardown

---

# モック関数

https://jestjs.io/ja/docs/mock-functions

> モック関数によりコード間の繋がりをテストすることができます。 関数が持つ実際の実装を除去したり、関数の呼び出し（また、呼び出しに渡されたパラメータも含め）をキャプチャしたり、new によるコンストラクタ関数のインスタンス化をキャプチャできます。 そうすることでテスト時のみの返り値の設定をすることが可能になります。

spyOn を使ってるのでこっち見たほうがいい  
https://jestjs.io/ja/docs/jest-object#jestspyonobject-methodname

---

# そもそもテストしやすいコードを書きたいよね！

以下を意識するとちょっと楽になるかも

- 参照透過性
- 純粋関数

---

# 参照透過性

同じ引数を渡したら、必ず同じ結果が返ってくるというもの(超ざっくり)
これを避けて参照透過であることを意識すると、
挙動が予測しやすいのでテストがしやすい

こんな感じ、必ず a, b の和を返却すると理解できる

```
const calc = (a: number, b: number) => {
  return a + b;
}
```

ちょっとした参考
https://qiita.com/Yametaro/items/1de3c2b76b8a4dc2d30d

---

# 純粋関数

副作用がなく参照透明な関数のこと

- 副作用とは
  関数が外部の状態を変えたり、外部に影響を与えたりすること

副作用が存在していると、挙動のパターンが増え、挙動というか考えることが増えて、テストがしづらくなる

ちょっとした参考
https://zenn.dev/miya_tech/articles/ec14ed8a731d1b

けっこう参考にした記事
https://qiita.com/Yametaro/items/ead024494f028e6183b7

---

# とはいえ

とはいえ、関数型プログラミングをしているわけでもないので、すべてにこれを要求するのは難しいところがある
なので、必要な時は副作用が発生したって問題がないと思う

ただ、楽だから副作用てんこもりでもいいや、みたいに書いてくとテストコードの時とか保守の時に痛い目を見る
（副作用てんこもりだと後からコードを見た人がそれに気づけない可能性や、読解のリソースをめちゃくちゃ使う）

なので、できるだけ「こうしたら後が楽かな？」「ああやったら単純にかけるかな？」みたな意識を持つところから始めたい