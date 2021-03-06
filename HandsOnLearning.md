# HandOnLearning

## 宣言型プログラミングと命令型プログラミングの違い
命令型はコードをみればやりたいことが一目でわかる。一方で宣言型は、「どのようにして結果を得るか」を重要視している。よって、命令型は必然的にコメントを書く必要がある。  

## 純粋関数
純粋関数とは、データをイミュータブル（変異しなこと）に扱う関数である。  
例えば、下記関数は、color_lawnにratingをべた書きしている。

```javascript
let color_lawn = {
    title: "lawn",
    color: "#00FF00",
    rating: 0
};

function rateColor(color, rating){
    color.rating = rating;
    return color;
}

console.log(rateColor(color_lawn,5).rating) //5
```

一方で、下記関数は新規に作成されたオブジェクトに対して変更を加え、元のオブジェクトのratingが変更されることはない、イミュータブルな関数である。
```javascript
const rateColor = (color,rating) => ({
  ...color,rating
  });
```

## map関数でオブジェクトのキーと値を配列に格納する

下記はmap関数を用いてオブジェクトを配列に格納している。
```javascript
const schools = {
    "York": 10,
    "Wasinton" : 2,
    "Wake": 5
}

const schoolArray = Object.keys(schools).map(key => ({
    name: key,
    wins: schools[key]
}));

console.log(schoolArray);

// [
//   {
//     name: "York",
//     wins: 10,
//   },
//   {
//       name: "Wasinton",
//       wins: 2,
//   }
//   {
//       name: "Wake",
//       wins: 5,
//   }
// ];
```

## reduceを用いて配列をオブジェクトに変換する

reduceを用いて配列をオブジェクトに変換する。結果は図1のようになる。  
reduceは第一引数にコールバック関数を、第二引数に初期値をもつため、今回は第二引数に初期値の配列`{}`を渡している。
```javascript
const colors = [
  {
    id: "xekere",
    title: "rad red",
    rating: 3
  },
  {
    id: "jbesof",
    title: "big blue",
    rating: 2
  }
];

const hashColors = colors.reduce((hash, { id, title, rating }) => {
  hash[id] = { title, rating };
  return hash;
}, {});

console.log(hashColors);

```
（図1）  
<img width="300" alt="image" src="https://user-images.githubusercontent.com/97214466/157387605-f3e1d88f-8772-45d7-b83d-12aa5308515e.png">  

## Babel
JavaScriptをコンパイルするためのツール。  
ES6は全てのブラウザで対応していないが、ES5はどのブラウザでもサポートされていたので、当時、BabelはES6をES5に変換するツールとして利用された.  

## useStateとuseRefの違い

useStateとuseRefの違い

useStateもuseRefも両方とも値を保持できるが、useStateはコンポーネントの再描画が行われる。  
```javaScript
import * as React from 'react';

const App: React.FC = () => {
  const [count, setCount] = React.useState(0);
  const countRef = React.useRef(0);

  return (
    <div>
      <div>カウント（useState）: {count}</div>
      <button onClick={() => setCount(count + 1)}>カウントアップ（useState）</button>

      <div>カウント（useRef）: {countRef.current}</div>
      <button onClick={() => countRef.current++}>カウントアップ（useRef）</button>
    </div>
  );
};

export default App;
```

## 制御されたコンポーネント、カスタムフック
Reactでは、フォームのデータはステートとして完全にReactにより制御されている。  
制御されたコンポーネントの中では、パフォーマンスの改善のために、重い処理を実行することは避けるべきである。  
`<input onChange={event => setColor(event.target.value)} type="color" required />`  
例えば、上記のようなinputタグが１個２個、１０個と増えていけば、上記のタグも増えていき、ステート値の更新処理が増えていく。
このような重複した処理を抽象化するために、カスタムフックが存在する。

## Propsバケツリレー
親から子コンポーネントCへのPropsの受け渡しが、親コンポーネント→子コンポーネントA→子コンポーネントB→子コンポーネントCとなると、BとCで無駄にPropsの再描画が行われる。  
この問題を解決するために、useContextを用いる。
まず、下記のように、index.jsで、適当なコンテキスト名を決めて、そのコンテキスト名にcreateContext()を適用させる。  

```javaScript
export const ColorContext = createContext();

ReactDOM.render(
  <ColorProvider>
    <App />
  </ColorProvider>,
  document.getElementById("root")
);
```
そして、ColorProvider.jsファイルをつくり、useContextをexportする。  
そして、子コンポーネントに受け渡したいPropsの情報を記述する。そして、ColorContext.Providerタグで囲えるようにchildrenを用意する。  
```javaScript

export const useColors = () => useContext(ColorContext);

(colorsやaddColors,removeColor,rateColorのロジック略)

  return (
    <ColorContext.Provider value={{ colors, addColor, removeColor, rateColor }}>
      {children}
    </ColorContext.Provider>
  );
```
そして、ColorProvider.jsの中から、使いたい情報を抽出すればよい。例えば。AddColorForm.jsというコンポーネントがあった場合、下記のように記述すれば、バケツリレーをしなくても、  
ColorProviderから必要な情報のみを抽出できる。  
```
export const AddColorForm = () => {

  const { addColor } = useColors();
```

## 副作用とは
Reactにおける副作用とは、描画の一部ではない処理のこと。  
例えば、チェックボックスに関する関数を作った場合、チェックボックスのUI構築に関する処理のみが記述されるべきである。よって、それ以外の処理は副作用となる。  
Reactはデータが更新される→コンポーネントが再描画される→副作用が実行される という一連のサイクルが繰り返し実行されている。  
useEffectを使うと、コンポーネントの描画後に副作用として実行される処理を記述できる。  

## 同一について
Javascriptでは、下記はtrueとなる。
```
const a = "a"
const b = "b"
a === b //true
```
しかし、下記の配列の比較ではfalseになる。これは、配列aと配列bは違うインスタンスであるとみなされているためである。  
```
const a = [1,2,3]
const b = [1,2,3]
a === b //false
```

## useCallback
useEffectでも不要な再レンダリングは防げるが、下記のようなコードの場合、wordsはコンポーネントが再描画されるたびに、新しい配列のインスタンスが生成されてしまい、useEffectの
依存配列でも、再レンダリングを防ぐことができない。

```javaScript
import { useEffect, useState } from "react";
import "./styles.css";

const useAnyLeyToRender = () => {
  const [, forceRender] = useState();

  useEffect(() => {
    window.addEventListener("keydown", forceRender);
    return () => window.removeEventListener("keydown", forceRender);
  }, []);
};

export default function App() {
  useAnyLeyToRender();
  const words = ["sick","powder"];
  useEffect(() => {
    console.log("fresh render");
  },[words]);                    /////////ここ
  return (
    <h1>Open the console</h1>
  );
}
```
こういった例外に対応するためにuseCallbackを用いる。  
例えば、下記の場合、コンポーネントが再描画されるたびに、関数fnは新しいインスタンスを生成するので、関数fnの再描画防止ができない。  
```javaScript
import { useEffect } from "react";

export default function App() {
  const fn = () => {
    console.log("hello");
  };

  useEffect(() => {
    console.log("fresh render");
    fn();
  },[fn]);

  return <div className="App"></div>;
}
```
そこで、useCallbackを用いて、fn関数をuseCallbackで囲うと、fnはuseCallbackにより”メモ化”され、関数のインスタンスが保存され、関数fnはコンポーネントの再描画のたびに、インスタンスが再生成されなくなり、結果として再描画の副作用を止めることができる。  
```javaScript
import { useCallback, useEffect } from "react";


export default function App() {
  const fn = useCallback(() => {
    console.log("hello");
  },[]);

  useEffect(() => {
    console.log("fresh render");
    fn();
  },[fn]);

  return <div className="App"></div>;
}
```
## メモ化とは  
>メモ化とは同じ結果を返す処理について、初回のみ処理を実行記録しておき、値が必要となった2回目以降は、前回の処理結果を計算することなく呼び出し値を得られるようにすることです。
>都度計算しなくて良くなることからパフォーマンス向上が期待できます。  


## useMemo
>useMemoは関数の結果を保持するためのフックで、何回やっても結果が同じ場合の値などを保存(メモ化)し、そこから値を再取得します。  
>不要な再計算をスキップすることから、パフォーマンスの向上が期待出来ます。  
>useCallbackは関数自体をメモ化しますが、useMemoは関数の結果を保持します。  


依存配列が空の場合は下記のように利用  

```javascript
const sampleMemoFunc = () => {
  const memoResult = useMemo(() => hogeMemoFunc(), [])

  return <div>{memoResult}</div>
}
```
依存配列に値が入っている場合は、その値が変わった時だけ関数を再実行したい場合は下記のように記述。
```javascript
const sampleMemoFunc = (props) => {
  const memoResult = useMemo(() => hogeMemoFunc(props.name), [props.name])

  return <div>{memoResult}</div>
}
```
