## 以下を使用して確認していく

https://playcode.io/javascript  
参考資料は mdn を主として確認してく
（あくまでも己の認識を展開するようなものなので、真実は mdn を見よう！）

## Array とは

https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array

> JavaScript の配列はリサイズ可能であり、異なるデータ型を交ぜて格納することができます。（これらの性質が望ましくない場合は、代わりに型付き配列を使用してください）。

- length から配列の拡張操作ができ、string も number も共存ができるということ
- `const sample = ["sample", 1, true];`← これも許容される

> JavaScript の配列は連想配列ではありません。配列の要素はインデックスとして任意の文字列を使用してアクセスすることができません。非負の整数（またはそれぞれの文字列表現）をインデックスとして使用してアクセスする必要があります。

- 連想配列とは https://wa3.i-3-i.info/word11931.html
- JS で連想配列を表現するということは Object の key に命名し、value に Array を加えるということ（なのかなぁ）

> JavaScript の配列はゼロオリジンです。配列の最初の要素は 0 の位置にあり、 2 番目の要素は 1 の位置にあるといった具合です。そして、最後の要素は配列の length プロパティの値から 1 を引いた位置になります。

> JavaScript の配列コピー操作はシャローコピーを生成します。（JavaScript オブジェクトに対するすべての標準組み込みコピー操作は、ディープコピーではなく、シャローコピーを作成します）。

- シャローコピー：コピーがコピー元のオブジェクトとプロパティにおいて同じ参照を共有する（同じ基礎値を指す）コピーのこと  
  https://developer.mozilla.org/ja/docs/Glossary/Shallow_copy
- ディープコピー：コピー先のオブジェクトのプロパティがコピー元のオブジェクトのプロパティと同一の参照（同じ値を指す）を共有しないコピーのこと  
  https://developer.mozilla.org/ja/docs/Glossary/Deep_copy

## 配列の生成

https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Indexed_collections#%E7%96%8E%E9%85%8D%E5%88%97

```
const arr1 = new Array(element0, element1, /* … ,*/ elementN);←実ははArrayをインスタンス化している
const arr2 = Array(element0, element1, /* … ,*/ elementN);
const arr3 = [element0, element1, /* … ,*/ elementN]; ←普段はこれを使っている
```

## 反復処理メソッド

https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array#%E5%8F%8D%E5%BE%A9%E5%87%A6%E7%90%86%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89

普段 map とか find とかしている時の中身

```
const sample = [1, 2, 3, 4, 5];
const result = sample.map((number, index, array) => {
  return {
    number: number * 2, // 処理中の要素
    index: index, // 処理中のインデックス
    array: array // メソッドが呼び出された配列
  }
});
console.log(result);
```

↑ 普段使っている map や find のコールバック関数はこういうのが入っている

---

## evrey()

Array.prototype.every()  
https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/every

> 列内のすべての要素が指定された関数で実装されたテストに合格するかどうかをテストします。これは論理値を返します。

```
const array1 = [1, 30, 39, 29, 10, 13];
console.log(array1.every((item) => item < 40));
// ↑array1はすべて40以下の数値なのでtrueを返す。これが39とかだとfalseを返す。
```

配列内の要素がすべて「そうだよね」という確認の際に使用する  
（いうてそこまで使った記憶はない）

```
const array1 = [];
console.log(array1.every((item) => item < 40));
```

気を付けないといけないのは、配列が空の場合は true が返ってくるということ。

> every は数学における「∀ （すべての / for all）」記号と同様のふるまいをします。  
> 特に、空の配列に対しては true を返します。（空集合のすべての要素が与えられた任意の条件を満たすことは空虚に真です。）

## filter()

Array.prototype.filter()  
https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/filter

> 指定された配列の中から指定された関数で実装されているテストに合格した要素だけを抽出したシャローコピーを作成します。

```
const words = ['spray', 'elite', 'exuberant', 'destruction', 'present'];
const result = words.filter((word) => word.length > 6);
console.log(result);
```

よく使うので覚えておいたほうが良い
これもコールバック関数を受け取るので、以下のような書き方もできる

```
const array = [-3, -2, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13];
function isPrime(num) {
  for (let i = 2; num > i; i++) {
    if (num % i === 0) {
      return false;
    }
  }
  return num > 1;
}
console.log(array.filter(isPrime)); // [2, 3, 5, 7, 11, 13]
```

↑ コールバック関数をアロー関数ではなくて関数宣言で判定する関数を作成し、これを与えることもできる

めったに使わないけど以下のような使い道もなくはない（避けるべきだけど）

```
const sample = [
  { no: 1, value: 'sample1' },
  { no: 2, value: 'sample2' },
  { no: 3, value: 'sample3' },
  { no: 4, value: 'sample4' },
  { no: 5, value: 'sample5' },
]

const result = sample.map((row) => {
  if (row.no % 2 === 0) { // 偶数の時だけ値を返したい
    return row
  }
}).filter((x) => x)
// mapは各要素で必ず何かを返すためundefinedが含まれる
// このfilterで実態があるもののみを返す
console.log(result)
```

↑ これやるなら最初に filter して処理すべき

## find()

Array.prototype.find()  
https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/find

> 提供されたテスト関数を満たす配列内の最初の要素を返します。 テスト関数を満たす値がない場合は、 undefined を返します。

```
const inventory = [
  { name: "apples", quantity: 2 },
  { name: "bananas", quantity: 0 },
  { name: "cherries", quantity: 5 },
];

console.log(inventory.find((item) => item.name === "cherries"));
```

これもよく使うので覚えておくべき
気を付けることは以下

- 見つかった最初の要素を返すので、テスト関数を満たす値が複数ある場合でも最初の一つしか返ってこない
- テスト関数を満たすものがなかった場合は undefined が帰ってくるので、これのケアが必要

## findIndex()

Array.prototype.findIndex()  
https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex

> 列内の指定されたテスト関数に合格する最初の要素のインデックスを返します。 テスト関数に合格する要素がなかった場合は -1 を返します。

find()のインデックスを返す ver、正直そこまで使った記憶ない

## flatMap()

Array.prototype.flatMap()  
https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap

> 最初にマッピング関数を使用してそれぞれの要素をマップした後、結果を新しい配列内に平坦化します。

```
const sample = [
  [1, 2, 3, 4, 5],
  [6, 7, 7, 9, 10]
];
console.log(sample.flatMap((row) => row));
// [1, 2, 3, 4, 5, 6, 7, 7, 9, 10]
console.log(sample.flatMap((row) => row.map((v) => v * 2)));
// [2, 4, 6, 8, 10, 12, 14, 14, 18, 20]
```

最初に配列の中の配列を平坦化するやつ

```
const sample2 = [
  [1, 2, 3, 4, 5],
  [6, 7, 7, 9, 10],
  [
    [ 11, 12, 13, 14, 15]
  ]
]
console.log(sample2.flatMap((row) => row));
// [1, 2, 3, 4, 5, 6, 7, 7, 9, 10, [11, 12, 13, 14, 15]]
```

↑ でも深さは 1 までしか平坦化しない  
使った記憶は（今のところない）  
いつ使うんだ？とググったらこんなのがあった  
https://zenn.dev/spacemarket/articles/51613197db688d

## forEach()

Array.prototype.forEach()  
https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach

> 与えられた関数を、配列の各要素に対して一度ずつ実行します。

```
const array1 = ['a', 'b', 'c'];
array1.forEach((element) => console.log(element));
```

もうそのまま、ただただ配列の反復処理  
気を付けないといけないのは forEach()は同期関数を期待するので Promise はもっていない。  
つまり forEach 内では（普通に書いたら）await は使えない  
https://zenn.dev/sora_kumo/articles/612ca66c68ff52

（そもそも forEach()を使用しないようなコーディングは心がけるべき）  
for...of, for...in と forEach()のどちらを使うべきか、みたいな話はいつだって生まれる  
プロジェクトに合わせればよいが、以下の文章の話が好み

> 「これ、どっちを使ったほうがいいんですか？」  
> 「まあ、あえていうなら forEach()かな。for...of は Airbnb の JavaScript スタイルガイドでも使うなっていわれてるし。  
> ただ 2 つを比べれば内部にミュータブルな変数が生じないのと、ここまで挙げた反復処理のメソッドとスタイルが共通しててメソッドチェーンでつなげられるから forEach がまだマシって程度のニュアンスでね」  
> 「本来ならどっちも使わないほうがいいと？」  
> 「うん。そもそも値を返さないこれらを使う目的って、主にその中で外部のミュータブルな変数を書き換えるといった副作用を起こすことだろうから、関数型プログラミングの文脈では往々にしてロジックの組み方をまちがえてるケースが多い。ほとんどの処理は map()や find()といった最初に挙げた値を返すメソッドでの組み合わせで十分なはずなので、どうしても他の方法が見つからなかった場合のみ forEach()を例外的に使うという感じだね」

参考：大岡由佳. りあクト！ TypeScript で始めるつらくない React 開発 第 4 版【① 言語・環境編】 (Kindle の位置 No.2865-2878). Kindle 版.

## map()

Array.prototype.map()  
https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/map

> 与えられた関数を配列のすべての要素に対して呼び出し、その結果からなる新しい配列を生成します。

```
const array1 = [1, 4, 9, 16];
const map1 = array1.map((x) => x * 2);
```

必ず使うやつ、覚える。
気を付けてるのは map 内で条件分岐によって return をした場合、偽の場合は「何もしない」ではなくて undefined を返すこと

```
const numbers = [1, 2, 3, 4];
const filteredNumbers = numbers.map((num, index) => {
  if (index < 3) {
    return num;
  }
});

// index は 0 から始まるので、 filterNumbers は 1,2,3 および undefined になります。
// filteredNumbers は [1, 2, undefined, undefined]
// numbers は [1, 2, 3, 4] のまま
```

あと一応 map 内でも非同期処理は書ける  
https://scrapbox.io/teamlab-frontend/Array.map()%E3%81%AE%E4%B8%AD%E3%81%A7%E9%9D%9E%E5%90%8C%E6%9C%9F%E5%87%A6%E7%90%86%E3%82%92%E8%A1%8C%E3%81%86

```
const urls = ['http:// ~~~', 'http:// ~~~'... ];
const fetching = urls.map((url) => hoge.fetch(url));
await Promise.all(fetching)
```

↑ こんな感じでの書き方は割と好き

```
const result = await Promise.all(records.map(async (row) => {
  const hogeName = await orm.fuga.findUnique({ row.id });
  return { // hogeNameをつかった何か }
}))
```

↑ こんな書き方はあまり好きではない  
最初から records の全量分の hogeName を findMany とかで取得したうえで、map 内では find とかすれば良いので  
※そもそもループ中に非同期処理をするべきではないんじゃないかなぁ...って思ってるけど上手な説明ができないので詳しく誰か教えてほしい ちょっとこのあたりは言語化するにはまだまだ修行が足りてないな～って書いてて思っている 個人的にはループ中に待機が発生する処理を書いたら性能が悪くなるとかくらいしか

## some()

Array.prototype.some()  
https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/some

> 指定された関数で実装されているテストに、配列の中の少なくとも 1 つの要素が合格するかどうかを判定します。配列の中で指定された関数が true を返す要素を見つけた場合は true を返し、そうでない場合は false を返します。それ以外の場合は false を返します。配列は変更しません。

```
const array = [1, 2, 3, 4, 5];
const even = (element) => element % 2 === 0;
console.log(array.some(even));
// true
```

配列の中にそれが 1 つでもあるよね？の確認の時に使う  
割と使った記憶がある

```
const sample = [
  { value: 1, success: true },
  { value: 10, success: true },
  { value: null, success: false },
  { value: 12, success: true },
]

const isMiss = sample.some((row) => row.success === false)
```

こんな感じでそのオブジェクトに成功可否を持っていた時に 1 つでも失敗があったら hoge するみたいな時の判別とか

---

↑ ここまでが反復処理メソッド  
↓ ここからはちょっと毛色が違うもの

---

## reduce()

Array.prototype.reduce()  
https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce

> 配列のそれぞれの要素に対して、ユーザーが提供した「縮小」コールバック関数を呼び出します。その際、直前の要素における計算結果の返値を渡します。配列のすべての要素に対して縮小関数を実行した結果が単一の値が最終結果になります。
> コールバックの初回実行時には「直前の計算の返値」は存在しません。 初期値が与えらえた場合は、代わりに使用されることがあります。 そうでない場合は、配列の要素 0 が初期値として使用され、次の要素（0 の位置ではなく 1 の位置）から反復処理が開始されます。

そもそも reduce の callBackFn は反復処理メソッドと違って以下のようなものをとる  
`reduce(callbackFn, initialValue)`

- callbackFn
  - accumlator 前回の callbackFn の結果
  - currentValue 現在の要素の値、初回は initialValue があれば array[0], そうでない場合は array[1]
  - currentIndex currentValue の位置
  - array reduce が呼び出された配列
- initialValue コールバックが最初に呼び出された時に accumulator が初期化される値

こんな感じになる  
`const hoge = fuga.reduce((accumlator, currentValue, currentIndex, array)=>{}, [])`

accumlator がプリミティブな値である場合は、prevValue(前の値) と表現するほうが適切かもしれない

```
const array1 = [1, 2, 3, 4];

// 0 + 1 + 2 + 3 + 4
const initialValue = 0;
const sumWithInitial = array1.reduce((accumulator, currentValue, index) => {
  console.log('acc', accumulator, `${index}回目`)
  console.log('cur', currentValue, `${index}回目`)
   return accumulator + currentValue
 }, initialValue);

console.log(sumWithInitial);
// Expected output: 10
```

https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce#%E5%88%9D%E6%9C%9F%E5%80%A4%E3%81%8C%E3%81%AA%E3%81%84%E5%A0%B4%E5%90%88%E3%81%AE_reduce_%E3%81%AE%E5%8B%95%E4%BD%9C

```
const array = [
  { key: '100', value: 1000, flag: true },
  { key: '101', value: 2000, flag: true },
  { key: '102', value: 3000, flag: false },
  { key: '103', value: 4000, flag: true },
  { key: '104', value: 5000, flag: true },
];

const resultR = array.reduce((acc, cur) => {
  return [
    ...acc,
    cur
  ]
}, [])

const resultM = array.map((row) => {
  return row
})

console.log(resultR)
console.log(resultM)
// これはまったく同じになる
```

↓ これがたまにある悩み  
https://qiita.com/TelBouzu/items/08ce089d2747d978fd0f

でも条件に合わないものを skip するだけなら flatMap でもやれるっぽい

```
const resultF = array.flatMap((row) => {
  if (row.flag) return row
  return []
})
console.log(resultF)
```

これは初めて知った  
flatMap の説明はこれをみる  
https://qiita.com/NeGI1009/items/4befe4695a4712d96d56

まぁ skip するだけなら filter + map でもいいかもしれない  
個人的には一つの高階関数でかけることを複数に分けるメリットがあまりないと思っちゃうので使わなかったりする。可読性を突っ込まれると弱い。

reduce は非同期処理で都合がいいことがかける  
https://penpen-dev.com/blog/async-await-reduce/  
正直非同期処理についての知識が足りなさすぎるので、勉強していきます。。  
この辺がそうだよねというやつ  
https://t-kojima.github.io/2018/07/18/0028-async-await-in-loop/

##

## 雑多な参考

破壊的なんとかとその回避方法  
https://zenn.dev/cryptobox/articles/ec99edaaa16b7b  
(push じゃなくてスプレット構文を使ってやるべきだったなぁって反省をしている)

reduce って実はなんでもできちゃうんだよな、という話  
https://alpacat.com/blog/array-reduce-with-async-function/

## おまけ

非同期処理とは  
非同期関数は，ECMAScript 2017 で導入された関数のようなもののひとつである。普通の関数定義に，
async をつけて記述すると非同期関数になる。非同期関数の内部では，await キーワードが使える。
非同期関数を実行すると，非同期関数の内部がまず実行される。非同期関数の内部では，await で非同
期処理の結果を待つことができる。非同期関数は await で非同期処理の結果を待つ間，実行を一時停止で
きる関数であるとみることができる。
ここで await で待つ対象となる非同期処理とは，Promise オブジェクトを結果として返すものであ
る。Promise オブジェクトとは，非同期処理の結果を表すオブジェクトである。非同期処理の結果がま
だ得られていないとき，Promise オブジェクトは pending 状態であり，非同期処理の結果が得られると，
fulfilled 状態となる。結果が得られると，Promise オブジェクトは結果の値を保持する。
await は，Promise オブジェクトに結果の値が得られるのを待ち，その結果の値を返す。この仕組みに
より，非同期処理の結果を待つこと，すなわち同期化ができる。
次に例を示す。sampleAsyncProc 関数は，1 秒後に結果の値 "result" が得られるようなダミーの非同
期処理である。非同期関数 afunc1 を実行すると，sampleAsyncProc の呼び出しが行われ，結果が得られ
るのを await で待つ。1 秒後，結果が得られ，その値 "result" を出力する。

```
// サンプルの非同期処理
// 1秒後に結果の値"result"が得られるPromiseオブジェクトを返す
function sampleAsyncProc() {
 return new Promise(function(resolve) {
 setTimeout(function() { resolve("result"); }, 1000);
 });
}
async function afunc1() {
 const res = await sampleAsyncProc(); // 非同期処理の結果を待つ
 console.log(res); // "result"
}
afunc1();
```

（参考：JavaScript 徹底攻略 関数 付録:圏論についての補足 57 ページ）
