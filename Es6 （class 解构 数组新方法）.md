
[TOC]
#Es6 （class 解构 数组新方法）
##JS中定义变量的方式
```
> ES5中用：var / function
> ES6中用：let / const / class / import ...
```
```
   
*let  1.没有变量提升 2.不可以重复声明3.定义的变量不会给window新增属性名
console.log(a);//报错
var a=1;
//let a;//'a' has already been declared 重复声明
console.log(a);//未定义

let b=1;
console.log('b' in window);//false

    *const:
     1)没有变量提升
     2)不可以重复声明
     3)定义的变量不会给window增加属性
     4)定义的是个常量,定义之后不可以修改 （let可以）
     5)一旦声明必须赋值 （let可以）
    //console.log(b);// b is not defined
    //const b=2;
    const c=2;
    //c=1;语法错误

```
###ES6和ES5的区别?
```
- 没有变量提升

 let a = 12;//=>声明一个变量a,并且赋值
 let a = 13;//=>声明一个变量a，此时a已经声明过了，所以会报错
 console.log(a);//=>Uncaught SyntaxError: Identifier 'a' has already been declared
* let虽然没有变量提升，但是有类似于ES5中，JS代码执行之前，自上而下进行查找的过程，在这个阶段，如果发现同一个作用域中，有重复定义的变量，直接抛出异常，阻止代码的执行

 //=>变量提升阶段：var b;
 var b = 13;//=>其实这是赋值操作
 var b = 14;
 console.log(b);//=>14

- 同一个作用域中不可重复声明

- 不会给window增加全局属性
 let a = 12;
// console.log(window.a);//=>undefined 注意是未定义 undefined

 var b = 13;
// console.log(window.b);//=>13
- 会形成块级作用域
- const设置的变量值是不可修改的（理解为常量）
- 暂时性死区
```
####块级作用域
```
概念：在Es6中除了对象等有特殊语法的大括号，剩下的大括号基本上都是块级作用域(块级作用域中使用LET声明的变量是私有的变量,不受外界干扰)
[ES5]
//1、全局作用域
//2、私有作用域
[ES6]
//除了全局和私有,新增块级作用域(隶属于私有作用域) ，在括号外面输不出变量的值
{
 var a = 12; 
}
console.log(a)//12；会输出里面的变量，不是块级作用域
{
    let a = 12; 
}
console.log(a);//=>Uncaught ReferenceError: a is not defined



if (true) {
    let temp = 12;
}
console.log(temp);//=>Uncaught ReferenceError: temp is not defined

for (let i = 0; i < 10; i++) {

}
console.log(i);//=>Uncaught ReferenceError: i is not defined
如果里面是var就输出十；

let obj = {
    //=>这里的不应该叫做块级作用域，它是单独的语法
};



```
- 暂时性死区
```
Es5检测一个在没有var 的变量a；
typeof a === 'undefined' ? console.log('ok') : console.log('no');//=>ok 在使用typeof检测一个没有声明过的变量,浏览器不会报错(暂时性死区)
Es6
 let a = 12;
 typeof a === 'undefined' ? console.log('ok') : console.log('no');//=>Uncaught ReferenceError: a is not defined (ES6规范中干掉了这个暂时性死区)


```
###class  （变量提升 继承 ...）
-  class 的constructor
```
*class A{
 //constructor代表当前类本身,如果你不写也会帮你加上一个空的constructor(){}
    constructor(){
        //this 当前实例,默认返回当前实例
        //如果你返回一个引用数据类型会改变实例的结果,如果是基本数据类型不会影响结果
        //return {name:"引用"};
    }
}
let a=new A();
//console.log(A.name);
console.log(a); //{}

*必须用new 类执行
class A{
    constructor(){
        this.a=1;
    }
}
//Class constructor A cannot be invoked without 'new'
//A();
//不可以直接当做函数执行,只可以使用new执行
```
- class的name问题
```
另一种写法 相当于 class ClassA{ }
let ClassA=class{
};
console.log(ClassA.name);//ClassA



let ClassA=class AA{
    getA(){
        //AA来代表当前类本身,但是只能在类里面使用,在外面是获取不到的（classA=AA）
        console.log(AA.name);
    }
};
console.log(ClassA.name);//AA
//AA is not defined
//console.log(AA); //在外面输不出AAname
let aa=new ClassA;
aa.getA();//"AA"  //在这里也可以输出name
```
- class的执行问题
```
class AA{
    constructor(name){  //接收传进来的参数
        this.name=name;
    }
}
let aa=new AA("nn");
console.log(aa);  // 实例上面有的属性 { name: 'nn' }

//采用class表达式让类直接执行
let a1=new class{
    constructor(name){
        console.log(name);
    }
}("name");   //name

```
- calss的变量提升
```
/function 会有变量提声
FF(); //输出ff
new FF();//输出ff
function FF() {
    this.f="ff";
}

//ES6 中的class跟了let 和const一样没有变量提升
//new GG;// GG is not defined
class GG{
    constructor(){
        this.gg="gg"
    }
}
```
- class的静态方法 strict

/类就相当原型,写在原型上的方法都被实例继承了,假如想给当前类本身加一些方法我们可以在方法前面加上一个关键字 static`,不会被实例继承,只有类本身可以使用`,例如 Array.of

```
class AA{
    constructor(){
        this.a="aa";
    }
    getA(){
        console.log("哈哈");
    }
    static getB(){
        console.log("我是AA的静态方法");
    }
}

let aa=new AA;
aa.getA(); //"哈哈"
console.log(aa.getB); //实例调用 undefined
AA.getB(); // 类调用  输出里面的内容
```
静态方法的继承     静态方法可以被子类继承
```

class F{
    static getF(){
        console.log("我是F的getF");
    }
};
class G extends F{
    static getF(){
        super.getF();  //super 是 F类
    }
}
G.getF();//"我是F的getF"
```

- 原型上的方法 不可以枚举  注意是方法（原型上的方法）
```
es5函数 枚举
function F() {
    this.f="ff";
}
F.prototype.getF=function () {};
let f=new F;
for (let key in f){
    //console.log(key);
    //f getF（this的私有属性 ，原型的公有方法都可以输出）
}


es6类 枚举
class FF{
    constructor(){
        this.ff="ff";
        this.getX=()=>{} //这不是一个方法，是一个属性
    }
    getF(){}; //原型上新增方法
}
let ff=new FF;
for (let key in ff){
    console.log(key); 
    //只会输出 ff getX
     // getF不会输出，是非枚举类型 是一个方法；
    
    
};
//检测是否可枚举
console.log(Object.getOwnPropertyDescriptor(FF.prototype,"getF"));
 //enumerable: false,
```
###class的继承  ( super与this问题)
```
class A{
    constructor(){
        this.a="a";
    }
    getA(){
        console.log(this.a);
    }
}
class B extends A{
    constructor(){
        //只要是写了constructor 必须要写super(),子类B中没有this 他的this的是来自父类A的this,只有执行了super();才会有this
        //this.b="b"; 在super之前不可以使用this
        let a=1;
        super();// ！！父类本身
        //A.prototype.constructor.call(this);
        this.b="b";
    }
    getB(){
        //在这super是一个对象,指向父类的原型
        super.getA()
    }
}

console.log(new B);
```
##解构赋值
###数组的扩展运算符
```
1、拓展运算符（解构赋值中）
// let ary = [12, 23, 34];
// let [a,...arg]=ary;
// console.log(a, arg);//=>12 [23, 34]

2、剩余运算符（函数参数处理）
// let fn = (context, ...arg)=> {
//     //=>剩余运算符：把函数传递的实参汇总为一个集合(数组)
//     //原因：箭头函数中不支持ARGUMENTS,想要获取实参集合,我们可以使用剩余运算符完成(...ARG)
//     console.log(context, arg);//=>Math [10,20,30]
// };
// fn(Math, 10, 20, 30);

3、展开运算符
//=>把一个数组中的每一项展开
//console.log(...[12, 23, 34]);//=>12 23 34
//console.log(...{name: '珠峰', age: 12});//=>Uncaught TypeError: undefined is not a function

```
```
=>获取数组最大值
// let ary = [12, 23, 34, 11, 16];
 console.log(Math.max.apply(null, ary));//=>34
 console.log(ary.sort((a, b)=>b - a)[0]);//=>34
 console.log(eval(`Math.max(${ary.toString()})`));//=>34
// console.log(Math.max(...ary));
```

### 函数参数 - 默认值、参数打包、 数组展开
```

```
###对象结构赋值
```
*变量名和属性名一样的情况
    //let{name,age}={age:10,name:"珠峰"};
   name,age 是变量

    //变量名不跟属性名一样的时候
    ///let {age:a,name:n}={age:10,name:"珠峰"};
   变量:a,n =》代替了age，name，只能输出a，n这些替代值，age，name不能输出

    //let {age:a,name}={age:10,name:"珠峰"};
   变量:a,name


*默认值
let {a:a1=10,b=b,c}={a:'m',b:"BB",c:"C"};
     console.log(a1);//‘m’ 不走默认值
     console.log(b);
     console.log(c);


let {a:a1=10,b=b,c}={b:"BB",c:"C"};
console.log(a1);//走默认值 a1=10
```
结构赋值案例 （构造相同的结构获取值）
```
let ary = [
    0,
    'ok',
    {
        id: 1,
        name: '张三',
        sex: 0,
        friend: [
            '李四',
            '王五',
            '尼古拉斯赵四'
        ]
    }
];
//=>获取第一项 0 和 第二项 'ok' (code msg)
//=>获取第一个人朋友中的最后一个 '尼古拉斯赵四' (lastFriend)
//=>获取第一个人的名字 (firstName)
//=> `获取数据状态：0=>ok 我的名字是：张三 我最后交的朋友是：尼古拉斯赵四`
let [code,msg,{name:firstName, friend:[,,lastFriend]}]=ary;
console.log(`result：${code}=>${msg} my name is：${firstName} my last friend：${lastFriend}`);
```

##箭头函数 
- 形式 
```
 //普通函数var fn= function fn(a) {};

   箭头函数let fn=(形参)=>{函数体}
  var a=1
    let fn=(a)=>{
       a++;
       return a;
   }
    console.log(fn(a));//2
    
   自执行  (()=>{
       console.log(this);
   })();
```
箭头函数简写形式
```
let fn = (a, b)=> {
    //a->10
    //b->20
};
fn(10, 20);

let fn = a=> {
    //...
    return a * 10;
};
fn(10);

let fn = a=>a * 10;
fn(10);

let fn = a=>b=>a + b;
var fn = function fn(a) {
    return function (b) {
        return a + b;
    };
};

```
- 箭头函数的this
```
    //箭头函数是没有this指向的，看函数*上级作用域的this*是谁，this就是谁 上级作用域（全局window  私有fn函数）
    
 *  (()=>{
    console.log(this);//上级window
   })();


    //一般给元素绑定事件的时候不会使用箭头函数,因为我们开发写业务逻辑的时候一般都是想给谁绑定的事件this就是谁
    
   箭头函数 h1.onclick=()=>{
        console.log(this);//箭头函数的this 是window 上级作用域是全局作用域
    };
    
  普通函数 h1.onclick=function () {
        console.log(this);//h1
    }


*举例
=>箭头函数中没有自己的THIS,它的THIS指向上级作用域中的THIS（指向上下文中的THIS =>指向宿主环境中的THIS）
let obj = {
    name: '珠峰',
    fn1(){
        //=>ES6中这样写,生成的函数是普通函数,不是箭头函数
        console.log(this);//=>this:obj
    },
    fn2: ()=> {
        console.log(this);//=>this:window  上级作用域是obj，在window中
    }
};
obj.fn1();
obj.fn2();
obj.fn2.call(obj);//=>this:window

```
- 箭头函数没有arguments
```

   let a=(...arg)=>{
        console.log(arg)//[]
    };
  a()
```
例题
```
*箭头函数与定时器
//=>真实项目中不要乱用箭头函数
//1、如果函数中不涉及THIS问题，用啥都可以
//2、如果涉及THIS问题，我们就要慎重了
//->外层函数基本上都不会使用箭头函数
//->内层函数中如果需要让this和外层函数保持一致,我们可以使用箭头函数
let obj = {
    name: '珠峰',
    fn(){
        =>this:obj
         let _this = this;  //将this存起来
         setTimeout(function () {
             =>this:window
             console.log(this.name);//=>我们想要获取OBJ.NAME,不能使用THIS
             console.log(_this.name);//=>珠峰
         }, 1000);

        setTimeout(()=> {
            //=>this:obj
            console.log(this.name);//=>珠峰
        }, 1000);
    }
};
obj.fn();





*function fn() {
       this.a=10; //winow.a=10
   }
   var obj={
       a:1,
       fn(){ //相当于 fn：function()
           var f=()=>{
               console.log(this.a);//obj.1 上级作用域 函数fn（）  obj.fn() 执行 有点所以是obj
           };
           f()
       },
       f:()=>{
           console.log(this.a);//window.a=10  //上级作用域是window 不是obj obj是一个堆内存空间，栈内存是是作用域
       }
   };
   fn();
   obj.fn();
   obj.f();
```
##Es6模板字符串拼接
```
模板字符串 ``
`xxx${JS CODE}...`

var ary=["one","n"];
var a=`<li>${ary[0]}</li>`;
    box.innerHTML=a;

```
##数据类型
###新增数组的方法Array
![Alt text](./es5.png)



forEach()  没有return   Es5
```
 
- 作用:数组每个元素都执行一次回调函数。
var ary=[1,2,"哈哈","1px",true];

   //第二个参数(可选的):是用来给第一个参数函数改变this用的
   ary.forEach(function (item, index, arr) {
       //数组有几项这个函数就执行几次,每一次都会默认传三个参数
       //item:当前项
       //index:当前项的索引
       //arr:原数组
      // console.log(item, index, arr); //1 0 (5) [1, 2, "哈哈", "1px", true] 内容 索引 数组
      
 console.log(this);  //输出五次[1, 2, "哈哈", "1px", true]
       
   ary，改变了它的this 有几项输出几次
      
   },ary);

/第二个参数是用来给第一个参数改变this用的
```

map()  和 forEach()的区别是它有返回值    Es5
```
- 作用:通过指定函数处理数组的每个元素，并返回处理后的数组。
- //map有返回值,返回值是里面的函数每一次执行的返回值组成的新数组
   var a=ary.map(function (item,index,arr) {
        //这里return的是什么,map返回值的数组中每一项就是什么
        return "哈哈";
    });
    console.log(a);
    
    var ary=[2,4,6,8];
    //[20,40,60,80]
    console.log(ary.map(function (item) {
        return item * 10
    }));

```
copyWithin(a,n,m) 从索引n找到索引m(不包括m),拿着这些内容从索引a的位置去覆盖,数组长度是不变,超出的内容不要
```
    var ary=[1,2,3,4,5,6,7];
    console.log(ary.copyWithin(2, 3, 5));
    //[1, 2, 4, 5, 5, 6, 7]
```
filter()   find()      findIndex()      Es6开始
- 作用:检测数值元素，并返回符合条件所有元素的数组。
```
     filter() 过滤 ,符合的留下
     //filter()
    var a=ary.filter(function (item,index,arr) {
        //这个函数也是数组有几项就执行几次,每一次执行根据返回值判断是否留下当前项,如果return true留下当前项item,否则过滤掉
        return typeof item=="number";
    });
    console.log(a);
    //求出数组中被3整除的所有项
    var ary=[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15];
    var arr1=ary.filter(function (item) {
        //return !(item%3);
        return item%3==0;
    });
    console.log(arr1);

     find()  从数组的第一项开始查找,只要函数的返回值是false就继续找,一旦函数的返回值是true就停止查找,返回数组的当前项
//    var ary=["哈哈",true,1,2,3];
//    console.log(ary.find((item, index, arr) => {
//        return typeof item=="number";
//    })); 


    // findIndex() 跟find方法一样只不过返回值是当前项的索引
    var ary=["",true,1,2,3];
    console.log(ary.findIndex((item, index, arr) => {
        return typeof item=="boolean";
    }));  //1  true是bolean类型
```
some()        every()
```
 some()  只要是有一个函数的返回值为true最终结果就是true
var ary=[1,2,3,"哈哈"];
console.log(ary.some((item, index, arr) => {
    return typeof item == "string"
}));//true
every()必须每一个函数的return都是true,结果才为true
```
 reduce()  reduceRight()
 作用:将数组元素计算为一个值（从左到右）（从右到左）
```
  // reduce()第二个参数是给prev设置初始值的
    var ary=[1,2,3,4,5];
    console.log(ary.reduce((prev, cur, index, arr) => {
        //prev:上一个函数的返回值,如果不再设置初始值,那么第一次的prev不算
        //cur:当前项
        //index:当前项的索引
        return prev * cur
    }));
    // prev    cur     prev + cur
    //         1       1
    //  1      2       1+2=3
    //  3      3       3+3=6
    //  6      4       6+4=10
    //  10     5       10+5=15

   

*var ary=[1,2,3];
console.log(ary.reduce(function (prev, item) {
    return prev*item;//10*prev 每一个上一项的返回值（10*1*2*3）
    return prev;//10
}, 10));


*类数组求和
 function fn() {
        console.log(this);
        return [...arguments].reduce(function (prev,item) {
            return prev+item;
        })
    }
//console.log(fn.call([],1, 5, 7, 8));
console.log(fn.apply([],[1, 5, 7, 8]));



 // reduceRight() 从右边开始
  var ary=[1,5,6,8];
    ary.reduce((prev,item,index)=>{
        console.log(item);//1,5,6 //没有传递prev ，最后一项默认为prev的值
    })


var ary=[1,5,6,8];
    ary.reduce((prev,item,index)=>{
        console.log(item);//1,5,6，8 //传了prev的值，最后一项就不被拿走了
    },10)

```
`Array类上的方法`
Array from()类数组转化数组,   Array of()一个数为数组,  isArray()
```
*Array.from 类数组转化成数组   + 放一个数组就会克隆一份
function sum() {
    console.log(Array.from(arguments)); //[1, 2, 3, 4, 5]
}
sum(1,2,3,4,5);

应用：new Set类数组 去重
var set1=new Set([1,2,3,1]);
    console.log(Array.from(set1));


*Array.of() 跟Array方法一样,只是解决了一个问题,传一个数字的时候是得到一个只有这个数字这一项的数组
   console.log(Array(6));// [empty × 6] 数组只传一个数，得到相应数的空位
    console.log(Array.of(9));//[9]
    console.log(Array.of(9,2,3,31));//[9, 2, 3, 31]

* //isArray()判断是不是一个数组 true/false
    console.log(Array.isArray([1, 2, 3]));
    console.log(Array.isArray("123"));
```
fill + 数组空位
```
*fill("字符串",n,m);将数组从索引n到索引m(不包括m)替换成"字符串"
    //初始化一个数组或者清空一个数组
    var ary=[1,2,3,4,5];
    console.log(ary.fill("嘿嘿",2,4));

    //得到有7个1的数组
    console.log(Array(7).fill(1));
*数组的空位
  
    var arr=Array(7);
    arr=[,undefined,,,,,,];
    //索引 in 数组:判断数组的这个位置是不是空位,空位:false,不是空位:true
    console.log(1 in arr);//undefined 不算空位  true

  //Es5对空位的处理不太相同,一般都是直接跳过,但是ES6中的方法都是将空位处理为undefined
    var arr1=[1,,,2,3];
    arr1.forEach(function (item) {
        console.log(item);
    });

    arr1.find(function (item) {
        console.log(item);
    })
```
####数组中的 for of循环  for in 对象的
```
var ary=["赵","钱","孙","李"];
    for (var item of ary){
        //item 当前项
        console.log(item);
    }
    //索引  ary.keys() 遍历索引的接口
    for(var index of ary.keys()){
        console.log(index);
    }
    */
    var ary=["赵","钱","孙","李"];
    //ary.entries(); 可以将索引和当前项一起遍历出来,放在一个数组中[索引,当前项]
    for(var [index,item] of ary.entries()){
        console.log(index,item);
    }
```


####数组的结构赋值
```
结构赋值：构建一个和变量相同结构的解构，快速获取对象或数组中的某一部分内容
//数组解构赋值的形式
    //[变量,变量]=数组
    let [x,y]=[1,2,3];
    console.log(x, y);

    //省略赋值
    let [x1,x2,x3]=[,"嘿嘿","哈哈"];

    let [,,...ary]=[1,2,3,4,5];
    console.log(ary);
    
    //不定参数
    let[a,b,..c]=[1,2,5,7,8]
    
    //默认值
    //只有付的值是undefined的时候才会走默认值
    var  a=[null];
    let [n=(function () {
        console.log("哈哈");
        return 1;
    })(),m]=a;
    console.log(n);//null
    
    //a[] /a[undefined] 输出默认值1
    
    

    let [[[[m1]]]]=[[[[1]]]];

    var arr=[[[[[[1,2,3],[4,5,6]]]]]];
    //"1,2,3,4,5,6"
    console.log(arr.toString());
    console.log(arr.join());
    console.log("" + arr);


    var s1=12;
    var s2=13;
    //交换位置
    [s1,s2]=[s2,s1];
    console.log(s1);
    console.log(s2);
```
###字符串的新增方法String
```
    *localeCompare  ,比较字符顺序
    //A在B的前面:-1  比较字母顺序
    console.log("a".localeCompare("b"));//-1
    console.log("b".localeCompare("a"));//1
    
    *includes:判断字符串中有没有指定字符,返回true/false
    //第二个参数(可选的,默认值是0):开始查找的索引
    var str="zhuf";
    console.log(str.includes("u"));//true
     var str="zhuf";
    console.log(str.includes("u",2));//true u在字符串中的索引是2，u后面的索引可以是0 1 2为true
  * 字符串复制 repeat()
    //参数小数:向下取整 3.9取 3
    //参数负数:报错   0 或 -0.（1-9）为空字符串  其它负数就报错
   
    var str="abc";
    console.log(str.repeat(2));//"abcabc"
    console.log(str.repeat(2.9));//"abcabc"
    console.log(str.repeat(-0.4));//""
    //console.log(str.repeat(-2));//报错
    
*  startsWith 字符串是不是以指定字符作为开始
   endsWith  字符串是不是以指定字符作为结尾
    //第二个参数:开始查找的索引
    var str="abc";
    console.log(str.startsWith("a"));
    console.log(str.startsWith("b",1));
    console.log(str.endsWith("c"));
    
 *     padStart(length,"字符串"):得到的新字符串的length, 放在字符串前面
       padEnd ES7中的方法       放在字符串后面
    var str="asd";
    console.log(str.padStart(5, "12345"));//12asd
    //从字符串"12345"中选出"12"放到str的前面
    console.log(str.padStart(5, "1"));
    //此时"1"放到str的前面还不够5个,就再放即可
    console.log(str.padEnd(10, "bcdgh"));//"asdbcdghbc"

```
  字符串方法不会改变原来的字符串 ， length不可变
  console.log(Object.getOwnPropertyDescriptor('str', length));
  console.log(Object.getOwnPropertyDescriptor([1,5], length));//数组
  ![Alt text](./查看length.png)



```
includes           数组中包含某一项，包含就返回true，不包含返回false
concat()	    连接两个或更多的数组，并返回结果。
copyWithin()	从数组的指定位置拷贝元素到数组的另一个指定位置中。
every()	        检测数值元素的每个元素是否都符合条件。
fill()	        使用一个固定值来填充数组。
filter()	    检测数值元素，并返回符合条件所有元素的数组。
find()	        返回符合传入测试（函数）条件的数组元素。
findIndex()	    返回符合传入测试（函数）条件的数组元素索引。
forEach()	    数组每个元素都执行一次回调函数。
map()	        通过指定函数处理数组的每个元素，并返回处理后的数组。
reduce()	    将数组元素计算为一个值（从左到右）。
reduceRight()	将数组元素计算为一个值（从右到左）。
some()	        检测数组元素中是否有元素符合指定条件。
valueOf()	    返回数组对象的原始值。

pop()	        删除数组的最后一个元素并返回删除的元素。
push()	        向数组的末尾添加一个或更多元素，并返回新的长度。
shift()	        删除并返回数组的第一个元素。
unshift()	    向数组的开头添加一个或更多元素，并返回新的长度。
indexOf()	    搜索数组中的元素，并返回它所在的位置。
join()	        把数组的所有元素放入一个字符串。
lastIndexOf()	返回一个指定的字符串值最后出现的位置，在一个字符串中的指定位置从后向前搜索。
sort()	        对数组的元素进行排序。
splice()	    从数组中添加或删除元素。
toString()	    把数组转换为字符串，并返回结果。
reverse()	    反转数组的元素顺序。
slice()	        选取数组的的一部分，并返回一个新数组。

```

##JS中的同步编程和异步编程
```
JS是单线程的（JS运行在浏览器中，浏览器只给`渲染执行JS操作`分配一个线程来处理，但是浏览器是多线程操作的）

单线程特点：一次只能干一件事，当前任务没有结束，下一个任务是无法进行操作的


同步编程：
  通俗：当前任务没有结束，下一个任务就无法执行

异步编程：
  通俗：当前任务处于等待执行状态，我们不等，继续执行下一个任务，当后续的任务完成了，我们才返回头执行这个之前等待的任务

JS中的异步操作：
1、定时器都是异步操作
2、事件绑定都是异步操作
3、AJAX中一般我们都采取异步操作（也可以同步）
4、回调函数可以理解为异步（不是严谨的异步操作）
剩下的都是同步处理

async(是否异步)：false（同步）/true（同步）
```
定时器中的同步异步
```
 *setTimeout(()=> {
    console.log('1');
}, 10);
console.log('2');
输出顺序//=>2 1
分析： 
  1、创建一个定时器（同步操作）
  2、10MS后执行匿名箭头函数（异步操作）
  3、输出外层的2 （同步操作）
  4、到了10MS后，执行之前等待的匿名箭头函数
  
  *setTimeout(()=> {
    console.log('1');
}, 0);//=>定时器等待时间设置为零，不是立即执行，每一个浏览器都有一个最小的反应时间（谷歌5~6 / IE 10~13），设置零，也需要等待一小段时间才能执行（异步操作）
console.log('2');
//=>2 1

*
 setTimeout(()=> {
    console.log('1');
}, 0);
for (let i = 0; i < 90000000; i++) {
    //=>在200MS循环期间内,我们什么事情都做不了,只能等循环完成后才能处理其它事情(JS是单线程)
    //=>定时器到达指定的时间也不一定执行(需要等其它任务完成后才可能会被执行)
}
console.log('2');
//=>2 1

*setTimeout(()=> {
    console.log(1);
}, 20);
setTimeout(()=> {
    console.log(2);
}, 10);
setTimeout(()=> {
    console.log(3);
}, 1000);
for (let i = 0; i < 90000000; i++) {

}
setTimeout(()=> {   //注意是个定时器，不是直接输出 先执行的for循环
    console.log(4);
}, 5);
//=>2 1 4 3
```
![Alt text](1515592699629.png)


