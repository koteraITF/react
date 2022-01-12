# React CSS modulesの使い方

## CSS modules
まずnode-sassをインストールする。<br>
そして、componentsフォルダーにCssModules.jsxとCssModules.module.scssを作成する<br>

CSSmodules.jsxファイルは以下のように編集する。<br>
divタグ内にclassName={class.クラス名}と定義する。

```
import classes from "./CssModules.module.scss";

export const CssModules = () => {
  return (
    <div className={classes.container}>
      <p className={classes.title}>- CSS Modules -</p>
      <button className={classes.button}>FIGHT</button>
    </div>
  );
};
```
そして、CssModules.module.scssでは、下記のように、普通のCSSファイルと同じようにコーディングできる。
```
.container{
  border: solid 2px #392eff;
  border-radius: 20px;
  padding: 8px;
  margin: 8px;
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.title{
  margin: 0;
  color: #3d84a8;
}

.button{
  background-color: #abedd8;
  border: none;
  padding: 8px;
  border-radius: 8px;
  &:hover{
    background-color: #46cdcf;
    color: #fff;
    cursor: pointer;
  }
}
```
