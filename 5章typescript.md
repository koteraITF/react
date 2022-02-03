# typescript

## 

```
let num:number = 0 ;　//文字
let str:string = "A";　//数字

let arr: Array<number> = [0,1,2] //数字配列
let arr2: number[] = [0,1,2] //数字配列

let tuple: [number, string] = [0,"A"] //tuple型

const funcA = (): void => { ~ } //関数void型

const obj { id: number, name: string } = {id: 0, name: "AAA" }; //オブジェクト型

```

引数に型を指定することで数字のみ渡すことも可能
```
  const calcTotalFee = (num: number) => {
    const total = num * 1.1;
    console.log(total);
  };
```
返り値をnumber型にしたい場合は下記のようにする
```
  const getTotalFee = (num: number):number => {   //返り値の型設定
    const total = num * 1.1;
    return total
  };
```

## config設定
ts.configのStrictをtrueにすると、関数や変数などを型指定しないとエラーが出るようになる。

## アロー関数

下記２種のコマンドは同じ意味になるが、{}を使う場合はreturnをかけるが、（）の場合はreturnを省略しなければならない。
```
const Component = () => {
  return <h1>Hello,World</h1>;
};

const Component = () => (
  <h1>Hello,World</h1>
);
```
## jsonplaceholderからのデータの取得

例えば、下記のようなjsonデータを取得したい場合を考える。
```
  {
    "userId": 1,
    "id": 1,
    "title": "delectus aut autem",
    "completed": false
  },
```

まず、下記のように設定することで、Jsonデータに型をつけると、データがどの型を受けるのかをTypescriptに理解させる。
```
type TodoType = {
  userId: number;
  id: number;
  title: string;
  completed: boolean;
};
```

そして、下記のように上記で設定した配列の型を設定することで、タイプミスを防ぐことができる。
```
export default function App() {
  const [todos, setTodos] = useState<Array<TodoType>>([]);　//配列の型を指定

  const onClickFetchData = () => {
    axios
      .get<Array<TodoType>>("https://jsonplaceholder.typicode.com/todos")　//配列の型を指定
      .then((res) => {
        setTodos(res.data);
      });
  };
```
## Propsの型定義
propsも下記のように型定義ができる。

```
type TodoType = {　　　////////TodoTypeの型定義
  userId: number;
  title: string;
  completed: boolean;
};

export const Todo = (props: TodoType) => {　　　//////////////propsの型定義
  const { title, userId, completed } = props;
  const completeMark = completed ? "[完]" : "[未]";
  return <p>{`${completeMark} ${title}(ユーザー：${userId})`}</p>;
};
```
また、TodoTypeの要素が、親コンポーネントにない場合はエラーが発生する。  
この場合は上記でcompleted:booleanという型定義しているにもかかわらず、completedを配置していないためエラーが発生する。
```
  <Todo title={todo.title} userId={todo.userId} /> 
  ///Property 'completed' is missing in type '{ title: string; userId: number; }' but required in type 'TodoType'.
```
しかし、下記のようにcompleted?とすることで親コンポーネントはcompletedを渡しても渡さなくてもどちらでもよくなる。
```
type TodoType = {　　　////////TodoTypeの型定義
  userId: number;
  title: string;
  completed?: boolean;    ///?
};
```
## 型定義の管理
配列の型定義はコンポーネントごとに書くのはめんどくさいので、typesフォルダーの中にtodo.tsのような形で保存して、exportするのが便利。

```
(todo.ts)
export type TodoType = {
  userId: number;
  id: number;
  title: string;
  completed: boolean;
};

```
また、todo.tsをpropsで受け取る場合は、下記のように指定するとよい。今回の場合はidはいらないので、Pickを用いる。  
Pick<取り出すTS,取り出したい型>   

```
export const Todo = (
  props: Pick<TodoType, "userId" | "title" | "completed"> ////Pick
) => {
  const { title, userId, completed = false } = props;
  const completeMark = completed ? "[完]" : "[未]";
  return <p>{`${completeMark} ${title}(ユーザー：${userId})`}</p>;
};

```

逆にOmitといういらない型のみを排除するコマンドも存在する。今回の場合はidを排除する。  
Omit<取り出すTS,除く型>  

```
export const Todo = (props: Omit<TodoType, "id">) => {　　　//////Omit
  const { title, userId, completed = false } = props;
  const completeMark = completed ? "[完]" : "[未]";
  return <p>{`${completeMark} ${title}(ユーザー：${userId})`}</p>;
};

```

## 関数コンポーネントの型定義

下記のようにVFC or FC を用いることで、関数であることを宣言できる。さらにVFCにPropsを渡すことができる。
```
type Props = {
  color: String;
  fontSize: String;
};

export const Text: VFC<Props> = (props) => {
  const { color, fontSize } = props;
  return <p style={{ color, fontSize }}>テキストです</p>;
};
```

## オプショナルチューニング

下記コマンドをApp.tsxにexportして、
```
export const UserProfile: VFC<Props> = (props) => {
  const { user } = props;
  return (
    <dl>
      <dt>名前</dt>
      <dd>{user.name}</dd>
      <dt>趣味</dt>
      <dd>{user.hobbies.join(" / ")}</dd>
    </dl>
  );
};
```

下記のようにApp.tsxをかくと、それぞれの属性が表示されるが、hobbiesを消すとエラーになる。
```
(App.tsx)
const user: User = {
  name: "じゃけぇ",
  hobbies: ["映画", "ゲーム"]  ///消すとエラーになる。
};
```
しかし、下記のようにuser.hobbies?のように？をつけることでその要素がなくてもエラーが出なくなる。  
これをオプショナルチューニングという。
```
export const UserProfile: VFC<Props> = (props) => {
  const { user } = props;
  return (
    <dl>
      <dt>名前</dt>
      <dd>{user.name}</dd>
      <dt>趣味</dt>
      <dd>{user.hobbies?.join(" / ")}</dd> ///オプショナルチューニング
    </dl>
  );
};
```


