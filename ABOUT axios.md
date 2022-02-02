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
