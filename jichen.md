[toc]
## 继承
- 子类继承了父类的一些特征，然后自己还有一套自己的特征

- 为什么要继承？
就是为了代码能够更好复用，组合起来生成一个新的类别
```
1、扩展式
child.prototype = {...Parent.prototype}

2、原型式

function paohui(){}
paohui.prototype = parent.prototype
child.prototype = new paohui

 3、拷贝:
child.prototype = deepclone(parent.prototype)
child.prototype = Object.assign(parent.prototype) 

4、寄生:
child.prototype = Object.create(parent.prototype)

5、类式
function Child(){
   Parent.call(this)
}

6、class式
class Child extends Parent {
 counstructor(){
 super()
 }
 }
```
### 类式继承
```
function Person(name,age){
        this.name = name;
        this.age = age;
        console.log(this);
    }
    function Coder(name,age,job){
        // console.log(this);
    //    Person(name,age);  //此处调用 Person，this指向window;并把Person的方法指向Coder
        Person.call(this,name,age);  //所以要用call改变this指向
        this.job = job;
    }
    let p = new Person('盘古','好几千年');//Person{}
    let c = new Coder('吕阔',20,'码农');
```

### 方法继承
Coder.prototype = Person.prototype 继承原型，这种方式不靠谱，， 
此方法相当于赋值，改变A会影响B

### 扩展式继承
- {...父类原型}
- Coder.prototype = {...Person.prototype}; 
开辟一个新地址，然后把Person.prototyp的内容放入新地址。这样互相改变不会影响彼此；
- 弊端：不能深度拷贝； 
**因为赋值的时候第一层为简单类型，简单类型的 
赋值就是赋值，如果第一层有引用类型，那么引用类型的赋值为赋址。**（拷贝继承可以解决此问题）
 - constructor的指向会被改变；

### 继承里的constructor
- 实例下的constructor == 实例的构造函数 
	- 但是这个constructor是随时随地随便可以修改的 
	constructor只能当做实例中指向构造函数的一种参考物 
	并不是能够左右实例的构造函数真相。
	- constructor什么时候会被修改呢？ 
	给构造函数的原型赋址对象的时候会变。
	- 解决：手动修正constructor指向
{ 
constructor:构造函数 
} 

### 拷贝继承
- JSON.parse(JSON.stringify(arr));经过处理，赋予新地址！！深度拷贝的技巧之一。
- 深度拷贝的递归方法
```
let arr = [1,2,3,4,[5,{ary:[{name:'小强'}]}]];
Object.prototype.xxoo = '哈哈';
function deepClone(obj){
//先声明一个数组，去存克隆出来的内容
//判断obj是否为数组，是数组就o就为[],否则为{}
let o = obj.push?[]:{};
//循环传进来的对象
for(let attr in obj){
// for(let i=0;i<arr.length;i++){
   //判断对象中的某个值是否为引用类型
   //如果是，就继续调用deepClone把引用值传到函数中
   if(obj.hasOwnProperty(attr)){
       if(typeof obj[attr] === 'object'){
           o[attr] = deepClone(obj[attr])
       }else{
           //如果是简单类型就直接赋值
           o[attr] = obj[attr];
       }
   }
}
return o;
}
 let arr2 = deepClone(arr);
```

### 原型继承
```
 fuction fn(){
    function 炮灰(){};
    炮灰.prototype = 父类.prototype;
    return new 炮灰; //返回的是一个实例化对象
}
子类.prototype = fn();        
```

### 寄生式继承
- Object.create({})，必须传入一个对象
- 返回值为一个新的对象，这个对象的原型链指向传入的参数
```
//寄生式继承
Coder.prototype = create(Person.prototype);
//手动修正指向
Coder.prototype.constructor = Coder;
```
### 混用式继承
- 原理：对象的属性只能有一个，如果写多个，下面的会把上面的覆盖。
- Object.assign(对象1,对象2,对象3….)
- 从后往前合并，改变第一个对象，第一个对象可以为{}
- 子类.prototype = Object.assign({},父类.prototype)
### class
```
class Fn {
    //利用constructor传参
    constructor(name,age){
        this.name = name;
        this.age = age;
    }
    //方法:  方法名 () {}
    say(){
        console.log(1);
    }    
}
let obj = {
    // fn:function(){
    //     console.log(2);
    // }
    //下面这种写法等同于上面这种写法
    fn(){
        console.log(2);
    }
}
obj.fn();
```
class继承
```
class 父类{
    constructor(参){
    }
    方法(){
    }
}
class 子类 extends 父类{
    constructor(参){
        //super下面才能写this，不然就报错
        //super括号中传入父类使用的参数
        super(参)
    }
    方法(){
    }
}

let 变量名 = new 子类(传入的实参);
```
```
//传参的方法之arguments
//缺点是不宜修改属性或方法，传入的实参必须对应
class 子类 extends 父类{
    constructor(){
        //arguments是数组，此处三点为拓展运算
        super(...arguments)
    }
    方法(){
    }
}
```
```
//传参的方法之剩余运算与扩展运算
//便于修改
class 子类 extends 父类{
    //constructor处的三点是剩余运算符，将传入的参数变成数组
    constructor(...参){
        //此处三点为拓展运算
        super(...参)
    }
    方法(){
    }
}
```
```
//三点之剩余运算符
//可以找开头
//此例中，得到 第一个参数和一个数组
function fn(a,...c){
     console.log(a,c);
}
fn(1,2,3,4,5);
```

### class的一些注意事项
- 静态方法，只给类自己用，不允许被继承
```
//设置静态方法
static shui(){
    console.log('睡觉');
}
```
- 不允许把一个新地址直接给类的原型
```
这种写法不可以！
Animal.prototype = {
      runing(){
          console.log(111);
      }
  }
```

