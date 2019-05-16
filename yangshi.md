
## 计算样式：
### getComputedStyle(element).属性
- 元素的style下的属性，默认为空字符串。
- 计算后的样式:
            **`getComputedStyle(element).属性`**
- 获取到的结果为**带单位**的字符串
                比如:100px
例：
```
#box{
    background: #000;
    width:200px;
}
 <div id="box"></div>
  document.onclick = function(){
        box.style.height = '100px';
        //获取box的宽度
        console.log(box.style.heigth);//100px
       console.log(box.style.width);//空
 console.log(getComputedStyle(box).width);//200px
        let w = parseInt(getComputedStyle(box).width);
        console.log( w -= 5 );//195
```
## 元素可视区宽、高度
###  ele.clientWidth/ele.clientHeight(支持padding,不包含边框)
 **`ele.clientWidth/ele.clientHeight`**
- 支持padding,不包含边框
- 元素可视区宽度  不带单位的数字    
例：
```
#box{
    background:pink;
    width:200px;
    height:100px;
    padding:100px;
    border:10px solid skyblue;
} 
<div id="box"> </div>   
 document.onclick = function(){
       console.log(box.clientHeight);//300     
    }       
```
### ele.offsetWidth/ele.offsetHeight(支持padding,也包含边框)
**`ele.offsetWidth/ele.offsetHeight`**
- 支持padding,也包含边框,不带单位的数字
例：
```
#box{
    background:pink;
    width:200px;
    height:100px;
    padding:100px;
    border:10px solid skyblue;
}
<div id="box"> </div>   
 document.onclick = function(){
       console.log(box.offsetHeight);//320     
    }       
```
>上面这2个，如果设置一个固定值，就以固定值为依据显示,不会以被内容撑开显示
        
### ele.scrollHeight/ele.scrollWidth(被内容撑开的高度(不包含边框))
- 被内容撑开的高度（不包含边框）
- 不管设不设置固定样式，都以被内容撑开为显示结果。
## 距离
获取的是不带单位的数字。
>如果要使用上面的属性，一定要做到以下几点
        1.子级有绝对定位
        2.定位父级也一定要有定位
        3.子级和父级都要有宽高（触发haslayout，zoom:1）
###  offsetParent:(定位父级)
- 定位父级
- 没有定位父级走body
### offsetLeft:(当前元素(左外边框)到定位父级的(左内边框)距离)
###  offsetTop:当前元素(上外边框)到定位父级的(上内边框)距离
例：
```
div{
    padding:100px;
}
#box1{
    background:tomato;
    position: relative;
}
#box2{
    background:gold;
    position: relative;
    border:10px solid #000;
}
#box3{
    background:firebrick;
    position: absolute;
    border:10px solid #000;
}
 <div id="box1">
        <div id="box2">
            <div id="box3"></div>
        </div>
    </div>
console.log(box3.offsetParent);//<div id="box2">...</div>
    console.log(box3.offsetLeft);//100
    console.log(box3.offsetTop);//100
```
## 绝对位置：
- 绝对位置:当前元素到页面顶端的位置。
###  clientLeft/clientTop 边框尺寸
-  getComputedStyle(box3).borderTopWidth 边框尺寸
###  getBoundingClientRect().(width/height/left/right/top/bottom/x/y) 当前元素到页面可视区的尺寸、距离
 - 注意:
 - 它是一个对象
 **`**是跟滚动条走的。`**
```
getBoundingClientRect().width/height/left/right/top/bottom/x/y
```
例：
```
 *{
        margin:0;
        padding:0;
    }
    div{
        padding:100px;
    }
    #box1{
        background:tomato;
        position: relative;
        margin-top: 100px;
    }
    #box2{
        background:gold;
        position: relative;
        border:10px solid #000;
    }
    #box3{
        background:firebrick;
        position: absolute;
        border:10px solid #000;
        margin-top:50px;
    }
 <div id="box1">
        <div id="box2">
            <div id="box3"></div>
        </div>
    </div>
let obj = box3;
    let t = 0;
    let l = box3.clientTop;//parseInt(getComputedStyle(box3).borderTopWidth)
    console.log(box3.clientTop);//10
    //判断当前obj是否不为null
    //box3 -> box2 -> box1 -> 
    while(obj){
    // t = 当前元素的上外边距 + 当前元素上边框
     t += obj.offsetTop + obj.clientTop
    //重新设置Obj是谁，让obj变为当前的定位父级
        obj = obj.offsetParent;
    }
    console.log(t-l);//360
```
函数封装
```
    class Tools {
        position(ele){ 
            let left = 0;
            let top = 0;
            let obj = ele;            
 while(obj){
      left += obj.offsetLeft + obj.clientLeft;
      top += obj.offsetTop + obj.clientTop;
          obj = obj.offsetParent;
            }
            left -= ele.clientLeft;
            top -= ele.clientTop;
            return {
                left,
                top
            }
 //如果对象中的key值和value值一样，那么可以简写为一个
              }
    }
   let t1 = new Tools;
    console.log(t1.position(box3).top);//360  //console.log(box3.getBoundingClientRect().hight);//220
    console.log(box3.getBoundingClientRect())//吃滚动条
    //DOMRect {x: 210, y: 60, width: 220, height: 220, top: 60, …}
```
## BOM:浏览器对象模型
- Borwser Object Model
###window.innerWidth/window.innerHeight: 获取浏览器的尺寸
**`window.innerWidth/window.innerHeight`**: 获取浏览器的尺寸
- 注意:
          window.innerWidth||window.innerHeight如果有滚动条，是忽略了滚动条的尺寸的
###  document.documentElement.clientWidth/Height:浏览器的尺寸,排除滚动条的
 **`document.body.clientWidth`**:浏览器的尺寸,排除滚动条的
 - 使用的时候把默认样式清除
例：
```
console.log(window.innerWidth);// 1280
console.log(document.body.clientWidth) //1263
```
###  window.open(url,用什么方式打开_blank,_self)
 -  window.open(url,用什么方式打开_blank,_self)
                打开新的窗口
 - 注意：
                    需要用户主动触发才不会被拦截
     - 比如：             
```
<a href="http://www.baidu.com" target="_blank">走百度</a>
 window.close();
    document.onclick = function(){
       window.open('http://www.baidu.com');
        window.close();
    }
```
###   window.close(): 关闭浏览器窗口
- window.close();
                关闭浏览器窗口
                最好也是用户主动触发体验才好
例：
```
<a href="http://www.baidu.com" target="_blank">走百度</a>
 window.close();
    document.onclick = function(){
        window.open('http://www.baidu.com');
        window.close();
    }
```
## 滚动条的距离
### window.pageYOffset/window.pageXOffset:只能读不能写
**`window.pageYOffset/window.pageXOffset`**
  - 滚动条的距离，只能读不能写
###  window.scrollTo(x,y) 专门用来写滚动条距离的
- **`window.scrollTo(x,y)`** 专门用来写滚动条距离的
###  document.documentElement.scrollTop:既能读也能写
- 滚动条的距离，既能读也能写(document.documentElement=HTML)
例：
```
 document.onclick = function () {
console.log(window.pageYOffset)//只能读不能写                                      
   window.scrollTo(0,0);
   //专门用来写滚动条距离的
 console.log(document.documentElement.scrollTop)  document.documentElement.scrollTop = 0;//既能读也能写
        }
```
##  onresize:缩放浏览器的时候触发的事件
例： 
```
window.onresize = function(){
        console.log(1);
    }
```
## onscroll:有滚动条的时候滚轮时触发
例：
```
<body style="height:3000px">
 window.onscroll = function(){
        console.log(8);
    }
```
## 浏览器内核信息
###  window.navigator.userAgent，查看用户的浏览器内核信息
  - 注意:
            使用的时候判断的字符串有可能会被模拟。
  -  操作系统的版本，位数
例：
```
console.log(window.navigator.userAgent);//Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36
```
## 浏览器地址栏信息
###  window.location.hash: ""  浏览器hash信息 #之后的信息，更换这个信息是不会刷新页面的
-  window.location.hash 即能读也能写
-  onhashchange:当hash值发生变化的时候就触发
例：
```
 document.onclick = function(){
        window.location.hash = 'a=3';
        console.log(window.location.hash);
    }
    //当hash值发生变化的时候就触发
    window.onhashchange = function(){
        alert('触发');
    }
```
### 其他:    
```
    host: ""  ip地址 + 端口号
    hostname: "" ip地址
    href: ""  url
    origin: "file://"
    port: "" 端口
    protocol: "file:"  协议
    reload: ƒ reload()   刷新页面
    replace: ƒ () 替换页面
    search: ""  查询信息
```
## 查询信息
###  window.location.href   在当前页面中跳转页面
###   window.location.search   ?到#号之间的信息
- 即可读也可写，只不过写的时候会刷新页面
 http://www.zhufengpeixun.cn?num=1#page=0
例： 
```
console.log(window.location.search);
 document.onclick = function(){
  window.location.search = 'code=1';
    }
```
## window.location.reload():刷新页面
例：
```
#box{
    width:100px;
    height:100px;
    border: 1px solid #000;
}
    <div id="box"></div>
    <button id="btn">刷新</button>
let ary = ['red','yellow','blue','green'];
    let num = 0;
    box.onclick = function(){
        num ++;
        num%=ary.length;
        box.style.background = ary[num];
    }
    btn.onclick = function(){
        window.location.reload();
    }
```
## 抖动小例子
```
#box{
    width:100px;
    height:100px;
    background: skyblue;
    position:absolute;
    top:0;
    left:300px;  
}
#box2{
    width:100px;
    height:100px;
    background:pink;
}
    <div id="box">
        <div id="box2"></div>
    </div>
    let ary = [10,-10,8,-8,6,-6,4,-4,2,-2,0];
    let num = 0;
    let timer = null;
    ox.onclick = function(){
    let l = this.offsetLeft;
    timer = setInterval(() => {
            num++;
 this.style.left = l + ary[num] + 'px';
          if(num == ary.length){
        clearInterval(timer);             
 box2.style.transition = '0.5s';
 box2.style.transform = 'scale(.5)';
                num=0;
            };            
        }, 16.7);
    }
```
## hash 小例子
```
<button id="prev">上一张</button>
<button id="next">下一张</button>
<img src="img/1.jpg" id="img"/>
<script>
    let arr = ['img/1.jpg','img/2.jpg','img/3.jpg','img/4.jpg'];    
    let num = 0;
   let hash = window.location.hash; //打开页面的时候获取hash值
    //看看有没有hash值
    if(hash){
        //获取hash值，把hash值设置为num
        num = hash.split('=')[1]*1;  //'[#a,1]'
    }else{
        window.location.hash = 'page=0'; //没有默认0
    }
    // console.log(num);
    img.src = arr[num]; 
    window.onhashchange = function(){
        img.src = arr[num];
    }
    next.onclick = function(){
        num ++;
        num %= arr.length;
        window.location.hash = 'page='+num;
    }
    prev.onclick = function(){
        num --;
        if(num < 0){
            num = arr.length-1;
        }
        window.location.hash = 'page='+num;
    }
```
##返回顶部
```
<style>
*{
    margin:0;
    padding:0;
}
#box{
    width:100px;
    height:50px;
    font-size:20px;
    text-align: center;
    line-height: 50px;
    color:#fff;
    background: skyblue;
    cursor: pointer;
    position:fixed;
    bottom:0;
    right:0;
    display: none;
}
body,html{
    height:3000px;
}
</style>
</head>
<body>
    <div id="box">返回顶部</div>
<script>
    window.onscroll = function(){
        // console.log(window.pageYOffset);
        if(window.pageYOffset >= 600){
            box.style.display = 'block';
        }else{
            box.style.display = 'none';
        }
    }
    let timer = null;
    box.onclick = function(){
        let t = window.pageYOffset;
        timer = setInterval(() => {
            t-=100;
            if(t <= 0){
                t = 0;
                clearInterval(timer);
            }
            window.scrollTo(0,t);
        }, 16.7);
        
    }
</script>        
```

 
           


