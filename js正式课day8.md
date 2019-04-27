##闭包   closure 
>函数套函数，子函数使用父函数的参数或者变量
  并且子函数被外界所引用，此时父级形成闭包环境
  父级的参数或者变量不被浏览器垃圾回收机制回收.
  此时，打印父函数的返回值，有个属性为Scopes
  Scopes下有个closure的属性，closure 就是闭包。
- 使用闭包可以一直存储父级的参数或者变量不被外界的函数或者变量所干扰（污染）
例：
```
 function fn(){
           let a = 0;
           function fn2(){
                 console.log(a);
        }   
           function fn3(){
        }
        console.dir(fn3);   
            return fn2;
    }
    let f = fn();
    console.dir(f);
```