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
