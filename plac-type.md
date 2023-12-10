## 勉強会について

- 知識の共有会であり、有識者がファシリになる必要はない
- あくまで勉強してきたことをお披露目して習慣的に学びましょうねの会
- かならず 1h で収める、超える場合は次回にする
- 全部を暗記する必要はない、そんなこともできるんだな程度に覚えて実践の際に思い出して、見ながら・質問しながら・教わりながらやれればよい
- 間違ってることもあるかもしれないので自分でも調べることは必要

## 以下を使用して確認していく

- プレイグラウンド  
  https://www.typescriptlang.org/play
- サバイバル TypeScript  
  https://typescriptbook.jp/
- 初めての TypeScript  
  https://github.com/oreilly-japan/learningtypescript-ja

※今回は書籍からの引用が多いので誤字脱字、段落があまいのは許してほしい

## TypeScript の特徴

そもそも TypeScript とはなんなのかをさらっと読んでいく  
https://typescriptbook.jp/overview/features

- TypeScript は静的型付けを持つ言語で、JavaScript は動的型付けを持つ言語である  
  両者ともデータ値そのものに型があることは共通しているが、静的型付け言語は変数や関数の引数および戻り値の型がプログラムの実行前にあらかじめ決まっていなければならない。動的型付け言語はそれらが実行時の値によって動的に変化するということ。

- ジェネリクスは型も引数のように扱うという発送

※なぜ JavaScript だとだめなのかとかとかなぜ TypeScript が必要なのかとか気になったら以下を読みはじめたらよい、と思う。。。

- TypeScript 誕生の背景  
  https://typescriptbook.jp/overview/before-typescript
- なぜ TypeScript を使うべきか  
  https://typescriptbook.jp/overview/why-you-should-use-typescript

## プリミティブ型

以下はすべてプリミティブ型

- boolean
- number
- string
- undefined
- null
- symbol
- bigint

- これらはすべて値を直接変更できない(immutable)  
  オブジェクトはこれに対して後から変更できる(mutable)

※ 値を直接変更できないのイメージ ↓

```
let x = 5;
x = x + 3; // 5という数値が変更されてるわけではない
console.log(x) // 8
```

- これらはプロパティを持たない  
  でも null と undefined 以外は length とか toString()とか使えて、オブジェクトみたいに扱える
  (オートボクシング)

- これ以外は全部オブジェクト、配列だってオブジェクト

## 合併型とリテラル型

https://github.com/oreilly-japan/learningtypescript-ja/blob/main/code/ch03.ts

### 合併型

表現する際は「|」を使う  
どちらか 1 つになるという型、合併型  
↓ こんなやつ

```
// stringかundefinedのどちらかが入る
// string | undefined
let mathematician = Math.random() > 0.5
    ? undefined
    : "Mark Goldberg";
```

```
// stringがnullのどちらかが入る
let thinker: string | null = null;

if (Math.random() > 0.5) {
    thinker = "Susanne Langer"; // Ok
}
```

### リテラル型

プリミティブな型をより限定的にする

```
// これはstring型ではなくHypatiaというリテラル型になる
const philosopher = "Hypatia";

// これはstring型になる
let philosopher = "Hypatia";
```

これは const(定数)は値を変えられない性質から、const に宣言と同時に代入すると Hypatia しか受け付けなくなるから

合併型とリテラル型を組み合わせることも可能

```
let lifespan: number | "ongoing" | "uncertain";

lifespan = 89; // Ok
lifespan = "ongoing"; // Ok

lifespan = true;
// Error: Type 'true' is not assignable to
// type 'number | "ongoing" | "uncertain"'
// 型 'true' を型 'number | "ongoing" | "uncertain"' に
// 割り当てることはできません。
```

初期値を持たない宣言の時はこういうことが起きる

```
// ↓こいつはstringを求めるけど初期値がないときはundefinedになる
let mathematician: string;
// なので　let mathematician: string | undefined と宣言しようね、という話
// (そもそもletを使わない)

// ↓stringしか宣言しないとここでerrorとなる
mathematician?.length;
// Error: Variable 'mathematician' is used before being assigned.
// 変数 'mathematician' は割り当てられる前に使用されています。

mathematician = "Mark Goldberg";
mathematician.length; // Ok
```

## 型エイリアス

合併型を繰り返し書くのが面倒だったり、長い合併型を使いたいときに登場する  
こういうやつ、いつも使ってるあれ

```
type RawData = boolean | number | string | null | undefined;

let rawDataFirst: RawData;
let rawDataSecond: RawData;
let rawDataThird: RawData;
```

慣例的に型エイリアスの名前はパスカルケースが使われる
合併型だけでなく、配列、関数、オブジェクト型も表現することが可能

補足：
型エイリアスは JavaScript ではない  
TypeScript は実行時に JavaScript にコンパイル(トランスパイル)されるが、型エイリアスはここには現れない  
TypeScript の型システムにのみ存在する、あくまで開発時の構成である

## オブジェクト型

https://github.com/oreilly-japan/learningtypescript-ja/blob/main/code/ch04.ts  
よく使うやつ

```
// こうすることで宣言することができる
let poetLater: {
    born: number;
    name: string;
};

// Ok
poetLater = {
    born: 1935,
    name: "Mary Oliver",
};

poetLater = "Sappho";
// Error: Type 'string' is not assignable to
// type '{ born: number; name: string; }'
// 型 'string' を型 '{ born: number; name: string; }' に
// 割り当てることはできません。
```

でも毎回こういう風に書いてると面倒くさくなる...
なので、前述した型エイリアスを用いる

```
// ↓こんな感じ、見慣れているやつ
type Poet = {
    born: number;
    name: string;
};

// 使う時は宣言した型エイリアスを指定する
let poetLater: Poet;

// Ok
poetLater = {
    born: 1935,
    name: "Sara Teasdale",
};

poetLater = "Emily Dickinson";
// Error: Type 'string' is not assignable to type 'Poet'.
// 型 'string' を型 'Poet' に割り当てることはできません。
```

ネストすることも可能

```
type Poem = {
    author: {
        firstName: string;
        lastName: string;
    };
    name: string;
};

// Ok
const poemMatch: Poem = {
    author: {
        firstName: "Sylvia",
        lastName: "Plath",
    },
    name: "Lady Lazarus",
};

```

でもこういう風に書くならネストされた箇所をまた型エイリアスで定義して、  
それを使用したほうが読みやすいね

```
type Author = {
    firstName: string;
    lastName: string;
};

type Poem = {
    author: Author;
    name: string;
};

const poemMatch: Poem = {
    author: {
        firstName: "Sylvia",
        lastName: "Plath",
    },
    name: "Lady Lazarus",
};
```

### オプションプロパティについて

```
type Author = {
    firstName?: string;
    lastName: string;
};
```

こんな感じに Key に?がつくやつ、これは undefined を許容する  
ただし string | undefined とは異なる  
前者は存在しなくても許容されるが、後者は存在は必要となる

### タグ付き合併型

革命かと思った、便利すぎる  
まぁ知らなかったし使ったことないけど  
調べてみて使えそうなら使っていきたいなあ

```
type PoemWithPages = {
    name: string;
    pages: number;
    type: 'pages'; // typeに値を指定する
};

type PoemWithRhymes = {
    name: string;
    rhymes: boolean;
    type: 'rhymes'; // こちらも同じように
};

// Poemは上記のどちらかになると宣言（合併型）
type Poem = PoemWithPages | PoemWithRhymes;

// poemはランダムな値によってPoemWithPagesかPoemWithRhymesになる
const poem: Poem = Math.random() > 0.5
  ? { name: "The Double Image", pages: 7, type: "pages" }
  : { name: "Her Kind", rhymes: true, type: "rhymes" };

// ここでpoem.typeで条件分岐をする
if (poem.type === "pages") {
  　// こちらに入るということはpoemはPoemWithPages型なのでpagesにアクセスできる
    console.log(`It's got pages: ${poem.pages}`); // Ok
} else {
    // そうでないならPoemWithRhymes型なのでrhymesにアクセスできる
    console.log(`It rhymes: ${poem.rhymes}`);
}
```

こういうのを型ガード(方の絞り込みのために利用できる論理チェック、らしい)と言ったりする
やらないとこうなる

```
poem.pages;
//   ~~~~~
// Error: Property 'pages' does not exist on type 'Poem'.
//   Property 'pages' does not exist on type 'PoemWithRhymes'.
// プロパティ 'pages' は型 'Poem' に存在しません。
//   プロパティ 'pages' は型 'PoemWithRhymes' に存在しません。
```

型ガードは typeof とかユーザー定義のやつとかあるので気になったら調べてみると良い
割とよくつかう、ここでは型の扱いに注目していきたいので割愛

### 交差型

「|」ではなく「&」を使う表現
既存の複数のオブジェクト型を組み合わせて新しい型を作成するのに使う

```
type Artwork = {
    genre: string;
    name: string;
};

type Writing = {
    pages: number;
    name: string;
};

type WrittenArt = Artwork & Writing;
// これは次と同じ
// {
//   genre: string;
//   name: string;
//   pages: number;
// }
```

交差型は合併型と組み合わせることができる

```
type ShortPoem = { author: string } & (
    | { kigo: string; type: "haiku"; }
    | { meter: number; type: "villanelle"; }
);

// Ok
const morningGlory: ShortPoem = {
    author: "Fukuda Chiyo-ni",
    kigo: "Morning Glory",
    type: "haiku",
};
```

ただ交差型はあまり積極的に使うべきじゃない、使うとしてもシンプルに使うべき  
型ガードしないといけないような場面も増えるし読み手に今はどっちの型がはいってるんだ、という負担を負わせることにもなる  
複雑になればなるほど型チェックのエラーメッセージも読みづらくなる

#### never 型

交差型を使うと生まれがちな存在

```
こういうやつ
type NotPossible = number & string;
```

number と string が 1 つの値に共存できるわけがないので、never と推論される

## インデックス型

https://typescriptbook.jp/reference/values-types-variables/object/index-signature

> オブジェクトのフィールド名をあえて指定せず、プロパティのみを指定したい場合

```
let obj: {
  [K: string]: number;
};
 
obj = { a: 1, b: 2}; // OK
obj.c = 4; // OK
obj["d"] = 5; // OK
```

## ジェネリクス（ジェネリック）

```
const identity = (input) => input
```

例えばこういう関数があったとき、input は任意の型になる

```
identity(1) // number
identity('sample') // string
```

こうなると事前に型を定めることができない  
なにも指定しなければ any となる  
こういう時に使うのがジェネリクスとかジェネリックとかいうやつ（generics）

```
const identity = <T>(input: T): T => input
```

型を引数のように扱って、それを T と定義しておく  
仮引数の中でこの T を使用したり関数内で使用することができる  
ただし、アロー関数の構文だと JSX の構文と競合するためデフォルトではエラーとなる  
※<>があると、閉じタグが必要になるので  
この時の回避方法が

```
const identity = <T = unknown>(input: T): T => input
```

あらかじめ unknown 型をデフォルトで設定しておくことで、これは jsx ではないと追加しておく  
これは TS に対して jsx ではなく型パラメータとして読み取るように指示してるだけなのでふるまいは変わらない、らしい。  
（バックエンドを開発してるときは React は存在してないのでまだ気にすることはない）  
TS のプレイグラウンドも設定で jsx を none にしたら普通に通る

これを踏まえたうえでこれを読んでみる  
どういうときにこれを使うのかがなんとなくわかる  
https://typescriptbook.jp/reference/generics

#### unknown 型

https://typescriptbook.jp/reference/statements/unknown
こういうやつ

## 型の再利用

https://typescriptbook.jp/reference/type-reuse

### typeof

与えられた値の型を返すやつ  
JavaScript の typeof とは異なる

```
const point = { x: 135, y: 35 };
type Point = typeof point;
/**
 * type Point = {
 *  x: number;
 *  y: number;
 * }
 */
```

### keyof

オブジェクトの型からプロパティ名を型として返すやつ

```
type Person = {
  name: string;
};
type PersonKey = keyof Person;
// name型
```

keyof, typeof を組み合わせるとちょっと便利なことができるようになる  
https://zenn.dev/harryduck/articles/9d09b1c133f9cd

### ユーティリティ型(utility type)

ちょっと解釈しきれていないところが多々あるので一緒に読む

#### Required\<T\>

https://typescriptbook.jp/reference/type-reuse/utility-types/required

> Required<T>は、T のすべてのプロパティからオプショナルであることを意味する?を取り除くユーティリティ型です。

#### Readonly\<T\>

https://typescriptbook.jp/reference/type-reuse/utility-types/readonly

> Readonly<T>は、オブジェクトの型 T のプロパティをすべて読み取り専用にするユーティリティ型です。

#### Partial\<T\>

https://typescriptbook.jp/reference/type-reuse/utility-types/partial

> Partial<T>は、オブジェクトの型 T のすべてのプロパティをオプションプロパティにするユーティリティ型です。

#### Record\<Keys, Type\>

https://typescriptbook.jp/reference/type-reuse/utility-types/record

> Record<Keys, Type>はプロパティのキーが Keys であり、プロパティの値が Type であるオブジェクトの型を作るユーティリティ型です。

#### Pick\<T, Keys\>

https://typescriptbook.jp/reference/type-reuse/utility-types/pick

> Pick<T, Keys>は、型 T から Keys に指定したキーだけを含むオブジェクトの型を返すユーティリティ型です。

#### Omit\<T, Keys\>

https://typescriptbook.jp/reference/type-reuse/utility-types/omit

> Omit<T, Keys>は、オブジェクトの型 T から Keys で指定したプロパティを除いた object 型を返すユーティリティ型です。

#### Exclude\<T, U\>

https://typescriptbook.jp/reference/type-reuse/utility-types/exclude

> Exclude<T, U>は、ユニオン型 T から U で指定した型を取り除いたユニオン型を返すユーティリティ型です。

### Extract\<T, U\>

https://typescriptbook.jp/reference/type-reuse/utility-types/extract

> Extract<T, U>は、ユニオン型 T から U で指定した型だけを抽出した型を返すユーティリティ型です。

---

使い道とか調べてみたら端的に書いてるところがあった  
https://gizanbeak.com/post/ts-utility-types
