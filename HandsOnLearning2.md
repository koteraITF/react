## useReducer

下記のようなチェック関数がある。  
ここで、`setChecked(checked => !checked)`のことをリデューサーという。  

```javascript
import React, { useState } from "react";

export const Checkbox = () => {
  const [checked, setChecked] = useState(false);

  const toggle = () => {
      setChecked(checked => !checked);
  }
  return (
    <div>
      <input
        type="checkbox"
        value={checked}
        onChange={toggle}
      />
      {checked ? "checked" : "not checked"}
    </div>
  );
};
```
さらに、useReducerを使えば状態管理を下記のように書き換えることができる。  

```
const [checked,toggle] = useReducer(checked => !checked, false);
```


## メモ化

下記のCat関数は親関数から呼び出されるたびにレンダリングが発生する。しかし、
memoを用いて、Cat関数をmemoで囲えば、不要な再レンダリングは防ぐことができる。
memoはCat関数のnameが変更するまで再レンダリングが行われない。
```js
import React, { memo } from "react";

export const Cat = ({ name }) => {
  console.log(`rendering${name}`);
  return <div>{name}</div>;
};

export const PureCat = memo(Cat); //メモ化
```

また、親関数が下記のように、アロー関数のPropsを渡せば、意図しない再レンダリングが発生する。
なぜなら、アロー関数は描画ごとにインスタンスを作成するからである。結果的にnameが変更されたと認識されるためである。
```
<PureCat name = {name => console.log(name)} />
```
その際には、memoに第二引数のpredicateを渡せばよい。  
下記のように、第二引数にnameの変化の有無を調べ、falseだったときにのみ、再レンダリングが行われる。つまり、nameに変化がない限り再レンダリングは行われない。  

`
export const PureCat = memo(Cat,(prev,next) => prev.name === next.name)); //メモ化
`
