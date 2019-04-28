[toc]
## this
###  this 为 window:
  - 1.直接在全局输出this
  - 2.函数打印this,并且直接调用
  - 3.定时器中普通函数this为window
  - 4.匿名函数自执行
###   事件中的this:
 - 哪个对象触发，this就是那个对象
###  实例:
  - new 构造函数 -> this就是实例
###   箭头函数:
-  this就走定义箭头函数的域
- 箭头函数不能new，一new就报错
- 箭头函数也没有arguments
### 对象中的this
```
let obj = {fn:function(){console.log(this)}}//object
```
```
 function fn(){
        return function(){
            console.log(3);
        }
    }
    fn.prototype.say = function(){
        console.log(2);
    }
    Function.prototype.say = function(){
        console.log(4);
    }
        let f = new fn;//new fn就等于函数调用
    console.log(f)// fn的返回值   ƒ (){console.log(3);}
    f.say()//4
//console.log(fn().say()===f.say());//true
```
```
 function Foo() {
         ngetName = function () {
                console.log(1);
            };
            return this;//返回的是实例.getName
        }
        //console.log(this);
        Foo.getName = function () {
            console.log(2);
        };
        Foo.prototype.getName = function () {
            console.log(3);
        };
        var getName = function () {
            console.log(4);
        };
        function getName() {
            console.log(5);
        }
        Foo.getName();// 2
        getName();//4
        Foo().getName();//1
        getName();//1
        new Foo.getName();//2
        new Foo().getName();//3
        new new Foo().getName();//3
```
##  为undefined的情况：
        1.对象没有属性的时候
        2.函数没有返回值
        3.形参没有实参
        4.变量没有赋值
        5.简单类型的自定义属性
## 出现null的情况 
		1.元素没有获取到。 
		2.在正则中搜索不到字符就会为null。 
		3.原型链的末端。 
