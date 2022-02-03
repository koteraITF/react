# axios

```
npm install axios
```
でaxiosをインストールし、下記コマンドでaxiosを利用できる。
```
import axios from "axios";
```

axios.get("URL")でURLのGETリクエストを取得できる。  
さらに、thenを用いることでコンソールに取得した情報を出力できる。
```
  axios.get("URL").then((response) => {   //responseは自由に命名できる変数
    console.log(response);
```
また、以下のような情報が得られた場合は、console.log(res.data)のようにすることで、responseの中のdata情報のみを抜き取ることも可能。
<img width="505" alt="image" src="https://user-images.githubusercontent.com/97214466/152123852-f53f342b-27c7-4d1c-bb05-b6645419851c.png">  

# axiosの具体的な使用例


下記はaxiosを用いて、Jsonplaceholder(https://jsonplaceholder.typicode.com/users)からデータを持ってきて、それをmap関数で解体して外部APIから情報を得る操作である。

## ---------------------------------------------------------------------------------
```
function App() {
  const [userProfile, setUserProfile] = useState([]);
  const onClickUser = () => {
    axios.get("https://jsonplaceholder.typicode.com/users").then((res) => {
      const data = res.data.map((user) => ({
        id: user.id,
        name: `${user.name}(${user.username})`,
        email: user.email,
        address: `${user.address.city} ${user.address.suite} ${user.address.street}`,
      }));
      setUserProfile(data);
    });
  };

  return (
    <div className="App">
      <button onClick={onClickUser}>データ取得ボタン</button>
      {userProfile.map((user) => (
        <div key={user.id}>
          <dl>
            <dt>名前</dt>
            <dd>{user.name}</dd>
            <dt>メール</dt>
            <dd>{user.email}</dd>
            <dt>住所</dt>
            <dd>{user.address}</dd>
          </dl>
        </div>
      ))}
    </div>
  );
}

export default App;

```
## ---------------------------------------------------------------------------------

具体的に解説をすると、  
まず、axios.getでURLをもってき、それをresという変数に代入する。
```
  axios.get("https://jsonplaceholder.typicode.com/users").then((res) => {
```
そして、一覧表示したいデータを変数として定義する。
```
const data = ~~~
```
さらに、dataに一覧表示したいデータを格納する。例えば、今回の場合はresの中のdataを持ってきたいきたいので  
下記のように設定する。
```
  const data = res.data.map((user) => ({
    id: user.id,
    name: `${user.name}(${user.username})`,
    email: user.email,
    address: `${user.address.city} ${user.address.suite} ${user.address.street}`,
  }));
```
そして、useStateを用いて、はじめにuseStateに空配列を用意して、ボタンが押されたときにデータを取得するように設定する。

```
  const [userProfile, setUserProfile] = useState([]);
  const onClickUser = () => {
  
   ///クリック処理の中身を記述する
   
      setUserProfile(data);
    });
  };
```
これでロジックは完成したので、＜button＞タグにonClickイベントをつけるだけである。よって、以下のようになる。
```
import axios from "axios";
import { useState } from "react";


function App() {
  const [userProfile, setUserProfile] = useState([]);
  const onClickUser = () => {
    axios.get("https://jsonplaceholder.typicode.com/users").then((res) => {
      const data = res.data.map((user) => ({
        id: user.id,
        name: `${user.name}(${user.username})`,
        email: user.email,
        address: `${user.address.city} ${user.address.suite} ${user.address.street}`,
      }));
      setUserProfile(data);
    });
  };

  return (
    <div className="App">
      <button onClick={onClickUser}>データ取得ボタン</button>
      {userProfile.map((user) => (
        <div key={user.id}>
          <dl>
            <dt>名前</dt>
            <dd>{user.name}</dd>
            <dt>メール</dt>
            <dd>{user.email}</dd>
            <dt>住所</dt>
            <dd>{user.address}</dd>
          </dl>
        </div>
      ))}
    </div>
  );
}

export default App;

```

