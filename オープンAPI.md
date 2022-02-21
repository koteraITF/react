# api

<img width="206" alt="image" src="https://user-images.githubusercontent.com/97214466/154915559-6c5becb8-c94b-43bf-8935-a5ac0d851a59.png">
上の写真のように、莫大なJsonデータ（760）から最新のデータ（760）を取り出したい場合は、下記コマンドのように、 [response.data.length-1] のように記述することで、  
最新の情報(760個目)を得ることができる。

```javaScript
  const getCountryData = () => {
    axios
      .get(`https://api.covid19api.com/country/${country}`)
      .then((response) => {
        console.log(response.data`[response.data.length-1]`
       );
      });
  };
```
