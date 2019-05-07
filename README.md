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
 把这个对象的地址，this -> 新对象。
 var this = new Object();

 最后我们会返回this出来给用户调用。所以我们在创建该对象的时候通过new Person()来创建。
### 视频7重点 面向对象-对象优化
1.函数体内部写返回this和不写的区别
``` 
function Person(name,age){
 this.name = name;
 this.age = age;
 return this;
}
```
```
function Person(name){
this.name = name;
return 'hhh';
}
```

1.如果不写，直接返回默认创建的新对象
2.如果返回this,直接返回默认创建的新对象,所以不要写return
3.如果返回基本数据类型，通过new对象创建出来的依然是person对象。
4.如果返回{},则返回的便是{}空对象，不是person对象。[1,2,3]也是返回[1,2,3]
4.1返回是对象，直接将这个对象返回给外界。
### 视频8
通过json对象传参。
```
//构造函数，option是object类型传进去。
function Dog(option){
this.name = option.name;
this.age = option.age;
}
```
对象的prototype方法，可以看成静态方法，定义在prototype上的方法是创建的对象全局共享的。
``` 
  function Dog(options) {
   this.name = options.name;
   this.age = options.age;
    }
    //prototype是公有的一些属性和方法，表示不需要重复创建的。所有对象都公共有的
    Dog.prototype.eat = function (something) {
        console.log(this.name + "吃" + something);
    }
    Dog.prototype.run = function (someWhere) {
        console.log(this.name + "吃" + someWhere);
    }

```
```
var dog = new Dog({age:18,name:"ckq"};
console.log(dog);//会创建出来
```
Dog.prototype来创建共有的属性和方法，这样就不用每个对象单独占有eat和run,而是共享一个。
或者直接将Dog.prototype方法重新声明一份来进行创建。
``` 
   Dog.prototype = {
        //初始化的方法也可以放到这里来
        _init:function(options){
          this.name = options.name;
          this.age = options.age;
        },
        eat:function (some) {
            console.log(this.name + some);
        },
        run:function (some) {
          console.log(this.name + some);
        }

    };
```
这样带来的隐藏风险是原先的constructor原型链也被干掉了。所以推荐使用
```
Dog.prototype.run = function() {
}
```
这样的方法来创建。
或者采用终极方法来创建
``` 
    //将原型链上所有的东西都删除掉
    function Dog(options) {
        // this.name = options.name;
        // this.age = options.age;
this._init(options);
    }
    Dog.prototype = {
        //初始化的方法也可以放到这里来
        _init:function(options){
          this.name = options.name;
          this.age = options.age;
        },
        eat:function (some) {
            console.log(this.name + some);
        },
        run:function (some) {
          console.log(this.name + some);
        }

    };
```
在Dog的prototype中也定义初始化的__init方法来进行创建。在构造函数中传入__init函数，将option选项传入即可。
### 视频10重点 计算器实现 通过面向对象的方法
```
var Caculator = {
        //结果
        result: 0,
        //行为 加减乘除 打印
        jia: function (num) {
            this.result += num;
            return this;
        },
        jian: function (num) {
            this.result -= num;
            return this;
        },
        cheng: function (num) {
            this.result *= num;
            return this;
        },
        chu: function (num) {
            this.result /= num;
            return this;
        },
        log: function () {
            console.log(this.result);
            return this;
        },
        clean:function () {
            //清零
            this.result = 0;
            return this;
        }
    };

```
因为每一个方法都返回了this,所以每个方法都返回了当前对象，所以可以进行链式调用，来进行下一步的操作。
```
 //链式调用
    Caculator.add(6).chu(2).log();
```
输出链式调用的结果。
### 视频24静态属性和方法重点
方法分为静态方法和实例方法，属性分为静态属性和实例属性，实例的是对象私有的，而静态的则是函数私有的，是全局共享的。
实例方法和属性举例
``` 
   function Person(name,age,doFunc) {
   //隐式会做     var this = new Object();
        this.name = name; // 实例属性和方法 绑定在对象上
        //静态属性，绑定在函数上的属性和方法 函数本质上是对象，所以可以动态添加属性和方法
        this.age = age;
        this.doFunc = doFunc;
        //隐式会做 return this;
        }
```
这里的name，age和doFuncs方法都是属于创建出来的new Person()对象的。
而Person也是一个对象，可以通过在Person上面添加属性和方法，创建静态属性和方法。
``` 
  if(!Person.personCnt){
            Person.personCnt = 0;
        }
        Person.personCnt++;
```
整个构造函数如下所示
``` 
  //构造函数设置属性和方法
    function Person(name,age,doFunc) {
   //隐式会做     var this = new Object();
        this.name = name; // 实例属性和方法 绑定在对象上
        //静态属性，绑定在函数上的属性和方法 函数本质上是对象，所以可以动态添加属性和方法
        this.age = age;
        this.doFunc = doFunc;
        //隐式会做 return this;
        //创建静态属性，刚开始肯定是没有的 如果没有就创建，否则在其基础上++
        if(!Person.personCnt){
            Person.personCnt = 0;
        }
        Person.personCnt++;

    }
```
而静态方法的添加最好在外部，不要在构造函数内部
``` 
  //创建静态方法
    Person.printPersonCnt = function () {
        console.log("总共创建了" + Person.personCnt + "个人");
    }
```
这样当创建了2个对象后，便可以通过调用静态方法打印输出有几个人
```
  var p1 = new Person("ckq",18,function () {
        console.log("张三在上课") //理解this是谁 实例属性和方法
    });
    var p2 = new Person("cpp",30,function () {
        console.log("李四在放羊")
    });
    p1.doFunc();
    Person.printPersonCnt();
```
### 视频25重点 内置对象的类型判断
可以使用typeof 查看基本数据类型 会输出object undefined string number boolean symbol
null的判断会输出object，对于对象{}和数组array的判断也会输出object.
对于对象类型,object 可以采用toString方法。会输出{object:object}，然而对于数组[1,2,3],则会输出1,2,3
```
   //或者采用toString的方法进行输出
    console.log(obj.toString());
```
所以以上方法不具有普遍性。
所以，最合适的方法是调用constructor.name次类型。表明构造的时候是什么类型的。
``` 
 //constructor 创建对象的构造函数
    //prototype 原型链
    console.log(arr.constructor.name); //打印出array
    console.log(obj.constructor.name);//打印出object
```
### 视频26重点 自定义类型的判断
首先定义两个类
``` 
    //获取自定义对象类型判断
    function Person(name,age) {
        this.name = name;
        this.age = age;
    }
    function Dog(name,age) {
        this.name = name;
        this.age = age;
    }
```
实例化对象进行判断
``` 
  //实例化构造 为对象
    var p = new Person("zs",18);
    var d = new Person("小花",8);
    console.log(p.toString());
    console.log(Object.prototype.toString(p));
    //constructor是对象的构造器，类似于一个产品上的标识器 获取类型 proto相当于原型链，对上一层逐层去找。
    console.log(p.constructor.name);
```
Object.prototype.toString(p);调用了老祖宗的方法，object对象上的，对象类型是[object,Object],而数组是[object,Array]这种类型的输出。
要获取到Person,也就是父亲一层的，调用p.construcor.name即可。相当于标识符，不需要向上面追溯到老祖宗。
### 视频27重点
instanceof作用 用来判断一个对象是否是属于一个类的 只要在构造函数的原型链上面，instanceof就返回true,否则返回false.
``` 
console.log(p instanceof Person);
```
判断p是否是Person派生的。若是全部判断是Object类型的，则都是true,判断是哪个派生器的，视情况而看。
``` 
var p = new Person();
var p = new Person; 
//以上两种都是返回this,指向当前对象
var p = Person(); //this会改变，指向的是window对象。
```
### 视频28重点 面向对象 访问函数原型对象
定义类 随后为原型对象添加方法
``` 
  //访问函数原型对象
    function Person(name,age) {
        this.name = name;
        this.age = age;
    }
    //原型对象 prototype
    Person.prototype.run = function () {
        console.log("22333");
    }
```
方式一可以用过原先类进行访问
``` 
 //方式1 通过构造函数拿到原型化对象
    console.log(Person.prototype);
```
方式二可以通过实例化类进行访问
``` 
 //方式2 通过实例化函数拿到原型对象
    var p = new Person();
    console.log(p);
    console.log(p.__proto__); //__proto__是一个非标准属性 ecmscript中没有这个属性，只是为了方便开发者调试的一个属性。
                              //建议在开发和调试阶段使用，正式上线时不建议使用。
```
打印出p或者通过调用p的__proto__属性，其不是一个标准属性，ecmscript中没有该属性。只是为了方便开发者。
可以修改p的__proto__指向，指向新的对象后，表示__proto__已经修改到了新的原型。
``` 
  //将指向修改掉 就是真正的修改掉了
    var newXY = {
        "add":function () {
            console.log("1")
        }
    };
    p.__proto__ = newXY;
    console.log(p.__proto__);
```
方法证明是可以的。
### 视频29重点 in和hasOwnProperty使用
in 是否用户某个属性，如果对象身上没有，则会去原型对象上查找
hasOwnProperty只会在原型对象内查找
``` 
  function Person(name,age) {
        this.name = name;
        this.age = age;
    }
    //原型对象上添加东西
    Person.prototype.address = '上海';
    //实例化Person
    var p = new Person("ckq",18);
    console.log("name" in p); //以字符串形式判断
    console.log("address" in p); //优先在自己，找不到再到原型对象
    //只在自己对象上，而不在原型对象上找
    console.log(p.hasOwnProperty("name"));
    //原型对象上是不会去找的
    console.log(p.hasOwnProperty("address"));

```


### 视频30重点 isPrototypeof和instanceof使用
instanceof 判断是否是原型对象上的构造函数创建的，**判断一个对象，是否是某个实例的原型对象**
isPrototypeof **判断一个对象，是否是某个实例的原型对象**

instanceof 判断某个对象是否是类的实例
```
function Person(name,age){
this.name = name;
this.age = age;
}
var p = new Person();
console.log(p instanceof Person);

```
isPrototypeof的使用，在原型对象上判断，是否是某个实例的原型对象
``` 
 function Person() {
    }
    //实例化出来的实例 对象
    var p = new Person();
    //是否是p的原型对象
    console.log(Person.prototype.isPrototypeOf(p));
    p.__proto__.isPrototypeOf(p); //拿到原型对象判断
```
拿到原型后进行判断。
### 视频31重点 constructor的使用
constructor就是一个标识器，标识这个对象的名字
``` 
  function Person() {

    };
    var p = new Person();
    console.log(p);
    //拿到实例化对象 真实产生的构造函数
    console.log(p.constructor.name);

```
如果修改了原型对象，则constructor会指向老祖宗object,导致错误使用。
``` 

    Person.prototype = {
     //   constructor:Person,//一定要把原型对象 一定要设置
        name:"cpp",
        age:18
    };
    var p1 = new Person();
    console.log(p1);
    console.log(p1.constructor.name);
```
所以此时要为原型增加constructor属性，指向Person。
``` 
  Person.prototype = {
       constructor:Person,//一定要把原型对象 一定要设置
        name:"cpp",
        age:18
    };
    var p1 = new Person();
    console.log(p1);
    console.log(p1.constructor.name);
```

