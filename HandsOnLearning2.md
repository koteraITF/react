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
