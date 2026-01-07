# JavaScript本格入門 - 理解のまとめ

## JavaScriptの本質

JavaScriptはプロトタイプベースのオブジェクト指向言語である。「プロトタイプベースの」とは、クラスに依らないでオブジェクトを作成および利用できることを意味する。全てのオブジェクトは最終的に`Object.prototype`に行き着くプロトタイプチェーンを辿ってプロパティを参照する。

JavaScriptは動的型付け言語であり、実行時に型が決定される。また、関数型プログラミングの側面も持ち、第一級関数として関数を扱うことができる。

---

## コア概念の体系

### 型システム

JavaScriptの型は、**基本型**（プリミティブ型）と**参照型**（オブジェクト型）に分類される。`typeof`演算子で型を確認できる。

**基本型（プリミティブ型）**
- `string`、`number`、`boolean`、`null`、`undefined`、`symbol`、`bigint`
- 値そのものが格納される
- `typeof`演算子では、`null`は`"object"`と表示される（歴史的経緯による）

**参照型（オブジェクト型）**
- オブジェクト、配列、関数など
- 参照（メモリ上のアドレス）が格納される
- 関数は`typeof`演算子で`"function"`として識別されるが、メモリ上では参照として扱われる
- 関数はオブジェクトの一種であり、プロパティを持ち、プロトタイプチェーンを辿ることができる

### 変数とスコープ

変数の宣言には`const`、`let`、`var`がある。現代的な開発では`const`と`let`が推奨される。

- `const`：再代入不可（定数）
- `let`：ブロックスコープを持つ変数
- `var`：関数スコープを持つ変数（ES6以前、現在は非推奨）

**スコープの種類**
- グローバルスコープ
- 関数スコープ
- ブロックスコープ（`let`、`const`）

**クロージャ**
- 関数が定義されたスコープの変数を参照できる仕組み
- データのカプセル化や状態の保持に利用される

### 関数

関数は参照型（オブジェクト型）に分類される。関数は入出力を定義した型であり、`typeof`演算子では`"function"`として識別される。メモリ上では参照として扱われ、同時にオブジェクトの一種でもあるため、プロパティを持ち、プロトタイプチェーンを辿ることができる。JavaScriptでは第一級関数として扱われる。

**関数の定義方法**
1. 関数宣言：`function 関数名() {}`
2. 関数式：`const 関数名 = function() {}`
3. アロー関数：`const 関数名 = () => {}`（ES6）
4. メソッド定義：オブジェクト内の`メソッド名() {}`

**関数の特徴**
- 引数と戻り値（`return`）で入出力を定義
- デフォルト引数、レストパラメータ（`...args`）が使用可能
- 高階関数（関数を引数や戻り値とする関数）として利用可能
- オブジェクトとしての側面：プロパティを持てる、`call()`、`apply()`、`bind()`などのメソッドを持つ

### オブジェクトとプロトタイプ

**オブジェクトとは**
JavaScriptにおいて、オブジェクトはハッシュ（連想配列）であり、プロパティとメソッドから構成される。メソッドは値として関数オブジェクトを格納したプロパティであるため、オブジェクトは本質的にプロパティの集合である。

**インスタンス化**
`new`演算子によってコンストラクタ関数を呼び出すことで、新しいオブジェクトインスタンスを生成する。

```javascript
const obj = new ConstructorFunction();
```

**プロトタイプチェーン**
- オブジェクトはプロトタイプチェーンを辿ってプロパティを参照する
- 全てのオブジェクトは最終的に`Object.prototype`に行き着く
- ES6以降は`class`構文も利用可能（内部的にはプロトタイプベース）

**オブジェクト操作**
- 分割代入：`const { prop1, prop2 } = obj`
- スプレッド構文：`const newObj = { ...obj }`
- `Object.assign()`、`Object.keys()`、`Object.values()`など

### 配列操作

配列はオブジェクトの一種であり、以下のメソッドが重要：

- `map()`：各要素を変換
- `filter()`：条件に合う要素を抽出
- `reduce()`：配列を一つの値に集約
- `forEach()`：各要素に対して処理を実行
- スプレッド構文：`[...array]`

---

## 非同期処理の体系

### 非同期処理の必要性

JavaScriptはシングルスレッドで動作するため、時間のかかる処理（ネットワーク通信、ファイル読み込みなど）を同期的に実行すると、UIがブロックされる。これを解決するために非同期処理が重要となる。

### 非同期処理の進化

1. **コールバック関数**（ES5以前）
   - 関数を引数として渡し、処理完了後に実行

2. **Promise**（ES6）
   - 非同期処理の結果を表現するオブジェクト
   - `then()`、`catch()`、`finally()`で処理を連鎖
   - `Promise.all()`、`Promise.race()`で複数の非同期処理を制御

3. **async/await**（ES2017）
   - Promiseをより直感的に記述できる構文
   - `async`関数内で`await`を使用

### Fetch APIとAjax

**Ajax（Asynchronous JavaScript and XML）**
- 非同期通信を実現する技術
- ページをリロードせずにデータを送受信

**Fetch API**
- モダンな非同期通信のAPI
- `fetch()`はPromiseを返す
- XMLHttpRequestの代替として使用される

```javascript
fetch(url)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

### エラーハンドリング

- `try-catch-finally`：同期的なエラー処理
- Promiseの`.catch()`：非同期処理のエラー処理
- `async/await`では`try-catch`が使用可能

---

## DOM操作とイベント処理

### DOM（Document Object Model）

DOMはHTMLを木構造として解析し、文字列としてではなくオブジェクトとして編集する仕組みである。

**DOM操作の基本**
1. **要素ノードの取得**
   - `document.getElementById()`
   - `document.querySelector()`、`document.querySelectorAll()`
   - `document.getElementsByClassName()`、`document.getElementsByTagName()`

2. **ノードウォーキング**
   - `parentNode`、`childNodes`、`nextSibling`、`previousSibling`
   - `children`、`firstElementChild`、`lastElementChild`

3. **要素の操作**
   - `textContent`、`innerHTML`：内容の取得・設定
   - `setAttribute()`、`getAttribute()`：属性の操作
   - `classList`：クラスの追加・削除
   - `createElement()`、`appendChild()`、`removeChild()`：要素の作成・追加・削除

### イベント処理

**イベントドリブンモデル**
- ユーザーの操作やシステムの状態変化に応じて処理を実行
- イベントリスナーを登録して処理を定義

**イベントリスナーの登録**
- `addEventListener(イベントタイプ, ハンドラ関数)`
- `removeEventListener()`：イベントリスナーの削除（メモリリーク防止に重要）

**イベントの伝播**
- **イベントバブリング**：子要素から親要素へ伝播（デフォルト）
- **イベントキャプチャリング**：親要素から子要素へ伝播
- `event.stopPropagation()`：伝播を停止

**イベント委譲**
- 親要素にイベントリスナーを設定し、子要素のイベントを処理
- 動的に追加される要素にも対応可能

**主要なイベントタイプ**
- `click`、`submit`、`input`、`change`、`focus`、`blur`
- `keydown`、`keyup`、`keypress`
- `load`、`DOMContentLoaded`

---

## モダンJavaScript開発

### モジュールシステム

**ES6モジュール**
- `export`：モジュールから値をエクスポート
- `import`：モジュールから値をインポート
- モジュールスコープを持ち、グローバルスコープを汚染しない

```javascript
// エクスポート
export const value = 10;
export function func() {}

// インポート
import { value, func } from './module.js';
```

**CommonJS**（Node.js環境）
- `module.exports`、`require()`
- ES6モジュールとは異なる仕組み

### テンプレートリテラル

- バッククォート（`` ` ``）で囲む文字列
- `${式}`で式を埋め込める
- 複数行の文字列を記述可能

### 分割代入

- 配列：`const [a, b] = array`
- オブジェクト：`const { prop1, prop2 } = obj`
- デフォルト値の設定も可能

---

## 画面開発への実践的応用

### フォーム処理とバリデーション

- フォーム要素の取得：`document.forms`、`form.elements`
- バリデーション：`required`属性、カスタムバリデーション関数
- フォーム送信の制御：`preventDefault()`でデフォルト動作を防止

### 状態管理

- コンポーネントの状態を管理する仕組み
- グローバル変数、クロージャ、イベントによる状態更新

### パフォーマンス最適化

- **メモリリークの防止**
  - イベントリスナーの適切な削除
  - 不要な参照の削除

- **DOM操作の最適化**
  - 頻繁なDOM操作を避ける
  - バッチ処理やDocumentFragmentの活用
  - 仮想DOMの概念（フレームワーク使用時）

### デバッグ

- `console.log()`、`console.error()`、`console.table()`
- ブラウザのデベロッパーツール
- ソースマップの活用
- ブレークポイントの設定

### エラーハンドリングの実践

- ユーザーフレンドリーなエラーメッセージ
- ネットワークエラーの適切な処理
- フォールバック処理の実装

---

## まとめ

JavaScriptは、プロトタイプベースのオブジェクト指向言語として、動的型付けと関数型プログラミングの側面を持つ。非同期処理（Promise、async/await）とDOM操作・イベント処理を組み合わせることで、インタラクティブなWebアプリケーションを構築できる。

画面設計・開発においては、以下の要素が重要：
- 非同期処理の適切な実装
- イベント処理とDOM操作の連携
- エラーハンドリング
- パフォーマンス最適化
- モジュール化による保守性の向上

これらの理解を深めることで、船舶関連の画面開発においても、堅牢で保守性の高いコードを書くことができる。
