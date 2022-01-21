

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

## ルーティング

下図のように、<BrouserRouter>タグで囲うことでタグ内にルーティングに関する設定を行うことができる.  
<Link>タグはタグ内に to="URL" を設定することで、Linkボタンを押したときに設定したURLに飛ぶことができる。しかし、これだけでは、URLが変わるだけで画面は変異しない。  
<img width="245" alt="image" src="https://user-images.githubusercontent.com/97214466/150472725-e7b74c55-013d-4a49-b12c-6b5afd28303a.png">  
  
下図のように<Switch>タグで囲い、その中に<Route>タグで囲ったものがページで表示される。 また、<Route>タグ内にpath="URL"を入れることで、そのURLに移動した際に画面も変異するようになる。  
 <img width="259" alt="image" src="https://user-images.githubusercontent.com/97214466/150473032-834ec9ad-df6b-48c4-95e5-35edaacf33f2.png">
  
  例えば、page1に飛ばしたい場合は下記のように設定すればよい。
  ただし、path = "" は部分一致なので、完全一致にしたい場合は、  exact path = ""のようにする。主にルートパスの設定で利用
  ```
    <BrowserRouter>
      <Link to="/page1">Page1</Link>
      <Switch>
        <Route path="/page1">
          <Page1 />
        </Route>
      </Switch>
    </BrowserRouter>
  ```
## ネスト化されたルーティング
ネスト化されたルーティングの設定は非常に難しい。
まず、Page1にPage1DetailAをネスト化させたい場合は下図のようにPage1.jsxファイルにLinkタグを用いてURLを設定する。  
  <img width="381" alt="image" src="https://user-images.githubusercontent.com/97214466/150475883-94e2d9ed-a31f-4e84-ae0d-a24513584d20.png">  
そして、下図のようにルーティングを設定していくのだが.....  むずいので少しずつ解説する  
  <img width="281" alt="image" src="https://user-images.githubusercontent.com/97214466/150475837-f60dd77f-f438-4f0b-b38f-f967255a04ab.png">
まず<Route>タグにrender関数をもたせる。
  ```
  <Route path="/page1" render = { () => (ルーティング記述) }
  ```
そして、ルーティング記述の欄にpage1とpage1DetailAのルーティングを記述する。
まず、<Route>タグの中に<Switch>タグを用いる。そして、その<Switch>タグの中で各種ルーティングを設定する。  
 下図は<Switch>タグの中の記述である。  
<img width="235" alt="image" src="https://user-images.githubusercontent.com/97214466/150476514-66ba7f26-6b76-4c79-8024-4e43d3ad1ef6.png">  
 また、応用として、render関数にはデフォルトでpropsが設定されているので、下図のように設定することでpage1のタイピングミスを防ぐことができる。　　  
    
 <img width="323" alt="image" src="https://user-images.githubusercontent.com/97214466/150476936-1fe3cab0-4e98-4914-a8c1-9e198afaf9d0.png">

    
    


  

  






