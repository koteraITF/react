

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

## useEffectについて
useEffectは再レンダリングを防ぐことができる。  
下記のように第２引数に配列を入れることで再レンダリングを防ぐ。  
```
useEffect(() => {
　再レンダリングを防ぎたい関数など
},[]);
```


例えば、下図のようにnumの再レンダリングを防ぎたい場合は、第二引数の配列にnumを入れる。
<img width="500" alt="image" src="https://user-images.githubusercontent.com/97214466/150306848-ba8f696a-faef-4edb-8751-a152aa6f5a61.png">  

また、useEffectは初期値としても利用でき、オープンAPIからとった情報をクリックイベントなどで登録しなくても、初期値として入れるだけで、  
画面にオープンAPIの情報が反映される。
```javaScript

 const [allCountriesData, setAllCountriesData] = useState([]);
  
 useEffect(() => {
    axios.get("https://api.covid19api.com/summary").then((response) => {
      setAllCountriesData(response.data.Countries);
    });
  });
```

## 再レンダリングが起きるとき
<img width="400" alt="image" src="https://user-images.githubusercontent.com/97214466/150468986-5146acb7-3819-44b9-9910-75f4ccd11a74.png">

## 再レンダリング対策

### memo
下図のように、memoで囲った部分はPropsが更新されない限り、レンダリングは起きない。  
<img width="280" alt="image" src="https://user-images.githubusercontent.com/97214466/150469424-f4b38943-f2b9-405a-b765-0e938dfa1ca8.png">

### useCallback
memoで囲っても、親コンポーネントから子コンポーネントにonClickなどの関数を与えれば、再レンダリングが走ってしまう。  
そのために、新しく親コンポーネントから子コンポーネントに対して関数を作る場合は、useCallbackを用いる。  
下図のようにuseCallbackで関数を囲み、引数に空配列を渡すことで、無駄な再レンダリングを止めることができる。  
<img width="271" alt="image" src="https://user-images.githubusercontent.com/97214466/150470437-029d2366-f967-4a52-876a-ed2e3966600c.png">

### chidren
const { children } = props;
下図のように、propsをchildrenに渡し、下記コマンドのようにすると、＜xxx>＜/xxx>で囲んだ部分はHTMLのタグのように使えることができる。また、＜xxx>＜/xxx>で囲むとxxxが適用されるようになる。
```
<xxx>{chidren}<xxx/>
```
 
  
<img width="718" alt="image" src="https://user-images.githubusercontent.com/97214466/150919892-6d5a2806-498b-4159-a0d1-2fe4bd36515b.png">

  






