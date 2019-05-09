

## let 与 var 的区别
let 一个变量只能声明一次
声明之后才能使用
不做window映射
支持块级作用域
## 数组
>  学习目标:
       每个方法一定要强行（理解）记忆，要知晓每个方法的使用规则
    尝试着自己手动实现一遍该方法。
     1.未来基本上都是操作数组
     2.数组的方法一定要牢记并且能够灵活应用
     
 - 数组写法:
            let arr = [ ]; 性能要高
            let arr = new Array( );
```
let arr = new Array();
let arr = [1,'好的',true,function fn(){}];
```
- 
	- 有length,既能读也能写
	- 数组中获取每个值通过下标去操作它 `[1,2,3] [[0][1][2]]`
	- 数组的最后一个一定是数组`.length-1`
	- 把数组清空，`arr.length = 0`;
- 要知道数组中要什么方法，直接console.dir数组
  去看__proto__下的方法即可。
### 数组方法
####  push(给数组的最后一位添加数据)
1.给数组的最后一位添加，一个或者多个数据，一个与一个之间用逗号分隔
2.**`返回值为新数组的长度`。**
例：
```
 函数调用还有函数调用，先算内部函数的调用
 如果有多个，从左往右运算。
  let arr = [3,214,35,43];
  arr.push(1,4,arr.push(8),4,arr.push(9));
  [3,214,35,43,8];  -> 返回值5
  [3,214,35,43,8,9] -> 返回值6
  [3,214,35,43,8,9,1,4,5,4,6]
   console.log(arr);
```
#### pop(往数组的最后一位删除一个数据)
  - pop(传参都是唬人的) 
  - 往数组的最后一位删除一个数据
  - **`返回值为:删除的那个`**
  例：
```
  let arr = [1,2,3];
 arr.push(5,arr.push(6),arr.pop(7),arr.push(arr.pop()));
     [1,2,3]
     arr.push(6) ->返回值 4
     [1,2,3,6]
      arr.pop(7) -> 返回值 6
     [1,2,3]
     arr.push(arr.pop()) ->返回值 3
     [1,2,3,5,4,6,3];
    console.log(arr);
```
#### unshift:(往数组的首位添加数据)
  - 往数组的首位添加一个或者多个数据
  - **`返回值是新数组的长度`**
 例：
```
let arr = [1,2,3];
  arr.unshift(4,5,6);
  arr = [4,5,6,1,2,3];
```
####shift:(往数组的首位删除数据)
  - shift(传参都是唬人的) 
  - 往数组的首位删除一个数据
  - **`返回值是删除的那个`**
例：
```
let arr = [1,2,3];
arr.unshift(3,23,arr.shift(15),arr.unshift(arr.shift()));
        [1,2,3]
        arr.shift(15) -> 返回值1
        [2,3]
        arr.unshift(arr.shift()) -> 返回值2
        [3,23,1,2,2,3]
```
```
let arr = [1,2,3];
    arr.unshift(3,23,arr.shift(15));
    // arr.shift(15) ->返回值 1
    // [2,3]
    //[3,23,1,2,3]
    
    arr.pop(arr.shift(arr.push(8)));
    //[3,23,1,2,3]
    // arr.push(8) -> 返回值6
    //[3,23,1,2,3,8]
    // arr.shift ->返回值 3
    // [23,1,2,3]
     
    arr.push(arr.pop(666));
    // [23,1,2,3] 
    // arr.shift()->返回值 23
    // arrr.pop -> 返回值3
    // arr.push ->返回值3
    // [3,1,2,3]  
   
 console.log(arr.unshift(arr.push(arr.pop(arr.shift())))); /4   arr.unshift()返回值是新数组的长度
console.log(arr); /[3,1,2,3]
页面自上而下运行 所以此时arr为计算完结果
``` 
#### splice(能够增删改查数组)
> 能够增删改查数组,根据参数的不同，结果就不同
```
 let arr = [23,213,12];
 arr.splice(0,2,212);
 从数组的第0位开始，删除23和213,替换成212
```
- 删除:(2个参数)  splice(x,n)
	- 第一个参数就是从数组的第几位起（选择数组的起始位置）  从0开始计数
	- 第二个参数就是操作几个数据(删除几个)
返回值就是删除的那几个
	- 修改原数组
例:
```
let arr = [1,2,3,4];
console.log(arr.splice(0,2));//从0开始，删除2个
返回值[1,2]
console.log(arr)//[3,4]
```
- 添加:(从哪开始添加,是否替换,添加的数据)
 splice(3,0,6,7)
	- 第一个参数就是从数组的第几位起（选择数组的起始位置）
	     从0开始计数
	- 第二个参数
	  就是操作几个数据(是否替换)
	  如果是替换，写替换几个数字
	- 第三个参数:(或者以上)
	   添加多个
例：
```
let arr = [1,2,3,5,6,7,8];
        从5开始，一个都不删除，添加6和7
        最后的结果是:
         [1,2,3,6,7,5,6,7,8]
        arr.splice(3,0,6,7);
```
-  替换
	- 第二个参数:  替换几个           
例：
```
 let arr = [23,213,12];
    从数组的第0位开始，删除23和213,替换成212
    结果:
        [212,12]
    arr.splice(0,2,212); 
```
 **注意:**
       **如果第2位为0，那么返回值为空数组**，
       **也就是说，第二位不为0，返回值就是你删除的那(几)个数据 返回的是*数组***。     
####forEach(专门用来循环数组的)
- 专门用来循环数组的。
- 两个参数:
	- 第一个参数:
		函数-> function(){}
		function(数组中的每个值,索引值,整个数组){  }
	- 第二个参数:
改变this指向,写啥是啥（如果写个null,undefined还是为window）
例：
```
 let arr = [true,'haha',10,{},[1,2,3]];
    arr.forEach(function(item,i,all){
         console.log(item);//数组中的每项
         console.log(i); //索引
        console.log(this);
    },arr);     
```
#### map:(循环数组,它的返回值为新的数组)
- 循环数组，它的返回值为新的数组
- 语法：
  function(item,i,all){
   return 新数组的每项}
   例：
```
<ul>     
    </ul>
let arr = [1,'你好','哈哈','呵呵'];
    let newArr = arr.map(function(item,i,all){
        // console.log(item,i,all)
        return '<li>'+ item +'</li>'
    });
    console.log(newArr);
    ul.innerHTML = newArr.join('');
```
#### join：(以某个字符串为连接符连接数组)
- 以某个字符串为连接符，连接数组的每一项
       最后返回一个字符串。
- 注意：
         如果不需要连接符，必须使用空字符串表示''
例：
```
let arr = ['你','好','吗']; 
console.log(arr.join(''));
```
#### filter(过滤条件成立的值)
 - filter过滤条件成立的值
 - 参数:
             function(item,i,all){
              return 条件成立的某项}
	 - 在回调函数中只有条件成立的结果才会被放到数组中。
	 - return的值，必须为true.
	 - 返回的是一个新的数组，这个新数组跟原数组是不一样的数组
例：
```
 //过滤大于28的数字
    let arr = [18,28,38,48,26];
    let arr2 = arr.filter(function(item,i){
        if(item > 20 && item <= 28){
            return item;
        }
        // return item > 20 && item <= 28;
    });
     箭头函数
     / let arr3 = arr.filter(e=>e > 20 && e <= 28);  
    console.log(arr3); //[28,26]              
```
例子需求： checkbox的勾选通过checked这个属性去修改 
需求: 
1.把下面的数组生成li input checkbox,并且通过 
布尔值来操作是否选中 
let i = false; 
```
<li><input type="checkbox" ${i?'checked':''}/></li>; 
```
2.当点击筛选的时候，过滤出已选中的数据，或者未选中的数据
```
 let arr = [true,false,false,true,false,true];
    //1.渲染页面
    function render(arr){
        let html = '';
        arr.forEach(function(item,i){
            html += `<li>
                <input type="checkbox" ${item?'checked':''}/>
            </li>`;
        });
        ul.innerHTML = html;
    }
    render(arr);
    go.onclick = function(){
        console.dir(o);
        //过滤出已选中
        if(o.checked){
            let arr2 = arr.filter(function(item,i){return item;});
            render(arr2);
        }
        //过滤未选中
        if(o2.checked){
            let arr3 = arr.filter(function(item,i){ return !item;});
            render(arr3);
        }
    }   
```
#### reverse:(翻转数组)
 - 翻转数组
       [1,2,3] -> [3,2,1]
 例：
```
 let arr = [1,2,3];
    arr.reverse();
    console.dir(arr); 
```
```
let str = '您迎欢峰珠'; //转成'珠峰欢迎您'   console.log(str.split('').reverse().join(''));
```
####  some(返回一个布尔值)
 - 查看数组中某项数据是否满足某个条件，
 - 只要有一个符合条件就返回true，
 - 如果所有项条件都不成立，返回false
 - 返回一个布尔值
例：
```
let arr = [1,2,3,4,5];
    //查看数组中是否有6，明显没有，就返回false
    console.log(arr.some(function(item){return item===6}))
```
####  every(判断数组中是不是每一项都符合某个条件)
- 判断数组中是不是每一项都符合某个条件
- 全部都符合返回true，只要有一项不符合就返回false
- 参数:
function (item,i,all){     }
例：
```
//就想知道，这个数组中是否所有项都为true
    let arr = [true,true,true,true,false];
    let a = arr.every(function(item){
        return item;
    })
    console.log(a);
   alert(arr.every(function(item){return item})?'都是true':'有内鬼');

    // let len = arr.length;
    // let num = 0;
    // for(let i=0;i<len;i++){
    //     if(arr[i]){
    //         num ++;
    //     }
    // }
    // if(num === len){
    //     alert('都是true');
    // }else{
    //     alert('有内鬼');
    // }
```
#### sort  排序
- sort默认排序是按照unicode编码来排序的
- 改变原数组  
- 也可以使用自定义排序
- sort中需要传入一个函数，a,b  
- 这个函数*必须*返回一个数字
- 是正数就交换位置，是负数就不交换位置
- a-b就是从小到大排序
  b-a就是从大到小排序
例：
```
let arr = ['2px',3,4,7,1,6,12];
    arr.sort(function(a,b){
        return parseInt(a)-parseInt(b);
    });
    console.log(arr);// [1, "2px", 3, 4, 6, 7, 12]
```

####  slice(截取数组)
slice(n,m ) :实现找到第n项到第m项(包括第n项和第m项)的内容，
返回一个新的数组(原有数组不变)
slice(m) : 从索引m开始，截取到末尾； 
slice( ):数组的克隆 slice(0); 
// 索引负数： 让当前length+负数；
- (包含起始位置,结束位置但不包含结束位置)
- 返回值为新数组
- 不会改变原数组。
例：
```
  let arr = [1,2,3,6,5];
   console.log(arr.slice(2,4));
   console.log(arr);//[3,6]
```

#### concat(链接数组 ) 
- 链接一个或者多个数组
- 返回值为新的数组
就算是没有数组链接
比如：
arr.concat( )  -> 克隆一份数组
例：
```
let arr2 = [1,2,3];
let arr3 = [4,5,6];
console.log(arr2.concat(arr3[7,8,9]));
 //[1,2,3,4,5,6,7,8,9]
```
#### toString(转字符串)
1) : 转字符串 
2) : 不需要参数 
3) : 返回一个去了中括号之后的字符串 
4) : 原有数组不变；
```
var ary = [1,23,8,9,90]
console.log(ary.toString());// "1,23,8,9,90"
console.log([12].toString());// "12"
console.log([].toString());// ""
console.log(ary);
```


