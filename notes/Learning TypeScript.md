# Learning TypeScript

## 一、TypeScript简介

### 1. 语言特性

 - <b><u>tsc</u></b>是TypeScript编译器的控制台接口，这个命令可以将TypeScript文件.ts编译成JavaScript文件.js。

   运行命令：```tsc index.ts```

 - npm是Node包管理的标准，并且是Node.js的默认包管理工具。

### 2. 类型

​	<b>可选的静态类型声明：</b>TypeScript允许明确地声明一个变量的类型，这种允许声明变量类型的功能就是被大家所熟知的<I>可选的静态类型声明</I>。

### 3.变量、基本类型和运算符

#### 1）基本类型有boolean、number、string、array、void和所有用户自定义的enum类型。所有这些类型在TypeScript中，都是一个唯一的顶层的Any Type类型的子类，any关键字代表这种类型。

| 类型    | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 只有两个值：true和false。  <br> ```var isDone:boolean = false;``` |
| number  | 和在JavaScript中一样，所有的数字在TypeScript中都是<b>浮点数</b>。<br>```var height:number=6;``` |
| string  | 表示文本，将使用的字符串放在双引号或者单引号中间。<br> ```var str:string="abc";```  <br> ```var str"string='abc';``` |
| array   | 表示数组。array类型的声明有两种写法：<br>a.在数组元素的类型后面跟着 <b>[]</b> ：```var list:number=[1,2,3];```<br>b.使用泛型数组类型 <b>Array</b> ：```var list:Array<number>=[1,2,3];``` |
| enum    | 枚举类型，可以用于命名数字集合，enum类型中的成员默认从0开始，也可以手动设置。<br>```enum Color {Red,Blue,White};```<br>```var c:Color=Color.Blue;``` |
| any     | any类型可以表示任意JavaScript值。一个any类型的值支持所有在JavaScript中对它的操作，并且对一个any类型的值操作时仅进行最小化静态检查。<br>```var notSure:any=4;```   ```notSure="maybe a string instread";```   ```notSure=false;```<br>```var list:any=[1,true,"free"];  list[1]=100;``` |
| void    | 在某种程度上，any的对立面就是void，即所有的类型都不存在的时候。一般出现在一个函数没有返回值时<br>```function warnUser():void{   alert("This is my wearning message");   }``` |

​		在JavaScript中还有两个原始类型：<b>undefined</b> 和 <b>null</b>。

 - <u>undefined</u> 是全局作用于的一个属性，它会赋值给那些被声明但未被初始化的变量。

 - <u>null</u>是一个字面量（不是全局对象的一个属性），它可以被赋值给那些表示没有值的变量。

     但是，在TypeScript中，不能将undefined和null当作类型使用，是 <b>不合法</b> 的。

#### 2）var、let和const

在TypeScript中，当声明一个变量时，可以使用 <b>var、let</b> 和 <b>const</b>关键字：

```typescript
var myNum : number = 1;
let isVaild : boolean = true;
const name : string = "zhangsan";
```

- 使用 <b>var</b> 声明的变量保存在最近的函数作用域中（如果不在任何函数中，则在全局作用域中）。
- 使用 <b>let</b> 声明的变量保存在最近的比函数作用域小的块作用域中（如果不在任何块中，则在全局作用域中）。
- <b>const</b> 关键字会创建一个保存在创建位置作用域中的常量，可以是全局作用域也可以是块作用域。这表明const是块作用的。

#### 3）联合类型

