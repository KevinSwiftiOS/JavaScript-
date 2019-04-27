### 闭包重点
1.如何产生了闭包 当一个嵌套的内部(子函数)引用了嵌套的外部(父)函数的变量的时候，就产生了闭包
2.闭包到底是什么
    理解一：闭包是嵌套的内部函数
    理解二：包含被引用变量的对象
    理解三：闭包存在于嵌套的内部函数中
3.如何产生闭包
 1.函数嵌套 2.内部函数引用了外部函数的数据(变量/函数)
4.闭包表现形式
  1.将函数作为另外一个函数的返回值
   2.将函数的形参作为实参传递给另外一个函数调用
5.闭包作用
 1.使用函数内部的变量在函数执行完成之后，仍然存活在内存之中，延长了局部变量的生命周期
 2.让外部函数操作，和读写函数内部的变量。
 
 经典案例: 重点掌握
``` 
    var btns = document.getElementsByTagName("button");
    for(var i = 0; i < btns.length;i++){
        var btn = btns[i];
        btn.onclick = function () {
            alert("点击了" + (i + 1) + "个");
        }
    }
```
这里都会出现5.因为没有绑定局部作用域。所以都绑定了最后一个
可以用闭包解决该问题：
``` 
    for(var i = 0; i < btns.length;i++){
        //通过局部i作为接受 现在自己作用域中查找，再去外面查找
        (function (i) {
            var btn = btns[i];
            btn.onclick = function () {
                alert("点击了" + (i + 1));
            }
        })(i); //这里是立马执行

```
将局部变量i传递进去，(function)(i)表示立即执行该函数。
随后点击每个弹出的都是对应的i.
表现形式 我们经常用的，将函数作为另外一个函数的返回值。
```
  function fn1() {
         var num = 10;
         function fn2() {
             num++;
             console.log(num);
         };
         function fn3() {
             num--;
             console.log(num);
         };
         return {
             fn2:fn2,
             fn3:fn3 //返回的是对象，表示2个函数都可以调用。
         }; //闭包函数返回才有意义
     } 

```
注意**只有在调用的时候才会形成闭包，此时还没有形成闭包**
```
var f =  fn1(); //通过全局变量的引用保存着值 局部函数执行完后还想再用它
     //闭包会产生内存空间的泄漏 所以最好在使用完成之后，进行释放 释放的函数为 让其指向一个空对象即可。
     f.fn2() //表示形成闭包。
```
当执行完成后，最好销毁掉f，否则会造成内存泄漏
``` 
f = null;
```
将内存销毁即可。 
外部函数操作内部函数的变量，也可以用到闭包。
1.内存溢出
一种程序运行出现的错误
当程序需要的内存超过了剩余的内存，就抛出内存溢出的错误
``` 
   //1.内存溢出
    var arrObj = {};
    for(var i = 0; i < 1000000000;i++) {
        arrObj[i] = new Array(1100000000);
    console.log(arrObj);
    }

```



2.内存泄漏
1.占用的内存没有及时的释放
2.内存泄漏多了容易导致内存溢出，内存不够用
2.2 常见的内存泄漏
 1.占用内存很大的全局变量
 ```
 var num = new Array(100000000);//开辟空间开的太多
```
 2.没有及时清理的计时器/定时器等
 ```
 var interid = setInterval(function(){
 console.log(111);
 },1000); //每隔1秒输出
 clearInterval(interid); //清理掉
```
 
 
 3.闭包
 造成内存泄漏 
 ```
 function fun1(){
 var num = 111;
 function fun2() {
 num--;
 console.log(num);
  }
  return fn2;
  }
  var f = fun1();
  f(); //全局范围内都是活的 造成内存泄漏
  f = null; //变成垃圾对象系统自动回收
  
```
### 视频4重点 异常处理
通过try catch来捕获异常，try里面是有问题的代码，catch来捕获异常，finally里是都会执行的代码。一般在finally中执行释放内存等操作。
``` 
 //异常抛出和捕获
    try {
        //可能出错的代码
        console.log(num);
        console.log(10);
    }catch (e) {
console.log(e);
    }
    finally {
        console.log(50);
    }
    console.log(20);
```
num未定义，会被抛出，在catch中捕获异常并且输出。
我们也可以自定义异常，当发现错误的时候throw抛出。
``` 
    //自己抛出异常
    function divide(a,b) {
        if(b == 0){
            //抛出异常
    throw '分子不能为0'
        }//分子为0
//正常执行
 return a / b;
    };
```
除法操作中，当b为0的时候，抛出异常。如果没有抛出异常，后面正常的执行a / b;
``` 
 try {
        divide(4,0);
    }catch (e) {
        console.log(e);
    }finally {
        //一般会在finally中释放资源。
    }
```
通过try/catch语句来捕获异常。这里会捕获异常，并且输出分子不能为0，一般在finally中执行释放资源操作。
### 视频6重点 面向对象
对象的创建，通过字典形式创建。
``` 
 //字面量创建对象 代码复用性差
    var p1 = {
        name:"张飒",
        run:function () {
            console.log(this.name + "跑");
        }
    };
    console.log(p1.run());
    console.log(p1.name);
    //通过内置构造函数创建
```
通过创建new object来创建
``` 
 var p2 = new Object();
    p2.name = "张三";
    p2.run = function () {
        console.log(this.name + "run");
    };//来做数据的传递

```
上述两种都有问题，没有抽离出公共的属性。导致复用性比较低。
所以可以通过自定义构造函数的方法，注意以下几点。
  1.函数首字母大写
  2.在创建过程中使用new关键字
  3.函数内部自动创建对象，赋值this指针
  4.自动返回创建出来的对象
 ``` 
  function Person(name,age,sex) {
      //var this = new Object();
         //指向新的产生对象 自动创建一个空对象
         //对象地址给了this this指向了新对象
         //2.this给空对象 绑定属性和行为
         this.name = name;
         this.sex = sex;
         this.age = age;
         //3 返回this return this
     }
     var p = new Person();
 ```
 在函数体内部，会自动创建一个this指针，自动创建this，后续我们给this绑定属性和方法。
 最后我们会返回this出来给用户调用。所以我们在创建该对象的时候通过new Person()来创建。
