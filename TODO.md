# javascript復習

## map関数

name は何の変数でも大丈夫
``` 
const nameArray = ["山田" ,"田中" , "木村"]

const nameArray = nameArray.map((name) => { 
  return name;
  })
  
  console.log(nameArray)
  /// 山田、田中、木村　
  ```
  また、map関数の第二引数にはindexを渡すことが出来る。
  ```
  const name = ["山田","田中","吉田"];
const nameArr = name.map((name,index)=> {
 
  console.log(`${index}番目は${name}です`);
} )

///0番目は山田です 
///1番目は田中です 
///2番目は吉田です 
  ```
  
  ## filter関数
  numは何の変数でも大丈夫
 
 ```

const numArr = [1,2,3,4,5];
const newNumArr = numArr.filter((num) => {
  return num % 2 === 0;
});
console.log(newNumArr)
/// 2,4
 ```
 
## 分割代入
```
const myProfile = { name: "山田" , age: 23 };
const message = 名前は{myProfile.name}です
```
これは冗長であるが、分割代入を利用することで
```
const myProfile = { name: "山田" , age: 23 };
const {name, age} = myProfile
const message = 名前は{name}です
```
と、変数が多くなるにつれて効率的にかける


 
 # React復習
 
 ## inputタグをuseStateを用いて状態管理する方法
 
 input1タグには valueをつけることに注意 また、状態を変更する際にはonChange関数を用いる。onChangeがないとinput欄に何も打ち込めない。
 ```
   const onChangeTodoText = (e) => setTodoText(e.target.value) 
   return(
    <input  value={todoText} onChange = {onChangeTodoText} />
   )
 ```
 
 ## inputタグに入力したものをTODOリストに追加する方法
 まず、buttonタグにonClick関数（クリックで何か起こす関数）を追加
 ```
  <button　onClick={onClickAdd}>追加</button>
 ```
 また、空文字はリストに登録されないように以下の設定をする。
 ```
 if (todoText === "") return;
 ```
 あとは、スプレッド構文を用いてtodoを追加し、set関数で値を更新する
 ```
  const onClickAdd = () => {
    if (todoText === "") return;
    const newTodos = [...incompleteTodos,todoText];
    setIncompleteTodos(newTodos);
    setTodoText("");
  }
 ```
 
 ## todoリストからtodoを削除する方法
 まず、buttonタグにonClick関数を用いる。そして、何番目のtodoを削除するか判断するためにindexの引数をわたす。
 そして、onClickDelete(index)のままだと、todoリストが追加された瞬間に全て削除されてしまうので、
 ()=> onClickDelete(index)のようにしてonClickDeleteをアロー関数内に入れる
 ```
  <button onClick={() => onClickDelete(index)}>削除</button>
 ```
 あとは、onClickDelete関数を定義するだけである。splice関数は第一引数に何番目の要素を消すか、第二引数に消す要素の数を指定する.
 
 ```
   const onClickDelete = (index) => {
    const newTodos = [...incompleteTodos];
    newTodos.splice(index,1);
    setIncompleteTodos(newTodos)
  }
 ```
## todoリストの完了リストから未完了リストへ移動させる方法
  
基本的なやり方は削除機能と同じなので詳しいやり方は省く

完了リストから削除
↓
未完了リストに削除した完了リストを移動させる

というやり方である。
onClick関数の定義は以下のようになるが、どの項目を消したかを明らかにするために、indexの引数は忘れないようにしよう。
incompleteTodos[index]など
```
  const onClickComplete = (index) => {
    const newTodos = [...incompleteTodos];
    newTodos.splice(index,1);
    setIncompleteTodos(newTodos)
    const newCompleteTodos = [...completeTodos,incompleteTodos[index]];
    setCompleteTodos(newCompleteTodos);
  }

````

## コンポーネント化
 App.jsのreturn以下が冗長なので、コンポーネント分割する。
 例えば、inputをコンポーネント分割する場合は、
 ```
export const Input = (props) => {
  const { todoText, onChange, onClick } = props;
  return (
    <div className="input-area">
      <input placeholder="TODOを入力" value={todoText} onChange={onChange} />
      <button onClick={onClick}>追加</button>
    </div>
  );
};
 ```
 上記を一つのファイルにまとめる。
 そして、App.jsに以下を記述し、importする。
 ```
 <Input  todoText={todoText} onChange={onChangeTodoText} onClick = {onClickAdd} />
 ```
 また、変数はprops化して渡す。
 その時に、分割代入を用いる。

propsは
コンポーネントファイルのonClickが実行→App.jsのonClickAddが実行
という流れになる。
