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
### 视频32重点 对象继承-原型链继承
继承:子类拥有父类的资源
01-原型链继承 02构造函数继承 03-组合继承 04-原型式继承 05-寄生式继承 06-寄生式组合继承 07-拷贝属性继承等
继承意义：减少代码冗余，方便统一操作。
继承不好：耦合性比较强。
每个函数都可以构造出一个对象，这个对象内部有属性指向这个对象的原型对象。__proto__
指向这个函数的原型对象。原型对象本质也是一个对象，也可以由另外一个构造函数生成，也指向构造函数的原型对象。
如下图所示：
<img src = "32.jpg" width = "50%" height = "50%"> 
arr对象有Array构造函数生成，arr的__proto__指向Array的原型对象，Array构造函数的prototype也指向Array的原型对象。
``` 
   var arr = [1,2,3];
    //产生的真实构造函数 拿到真实的构造函数的名字
    console.log(arr.constructor.name);
    //通过实例对象拿到原型 使用__proto__
    console.log(arr.__proto__);
    //通过构造函数拿到原型链 通过构造函数拿到原型链使用prototype
    console.log(Array.prototype);
    //验证 通过arr的__proto__的constructor也可以拿到构造函数
    console.log(arr.__proto__.constructor.name);
```
可查看结果。
### 视频35重点
对象的属性和方法，访问对象的属性和方法的时候，先查找有没有对应的实例属性和方法，如果有就直接调用，没有就去该对象的原型对象上去找，如果有就直接调用，如果没有就再去原型对象的原型对象上去找。
最终找到object.prototype,就是function，如果找不到返回null.
``` 
  function Person(name) {
        this.name = name;
        //重写toString
        this.toString = function () {
            console.log("我是增加在对象上的方法");
        }
    }
    Person.prototype.toString = function () {
        console.log("我是增加在原型对象上的方法");
    }
    var p = new Person("张三");
    p.toString();
    console.log(p);
```
重写了toString方法后，当实例化类的时候，调用的是子类的toString方法，而原型对象上的prototype方法并不会调用。
原始继承 sutdent的原型对象指向person的原型对象
``` 
student.prototype = person.prototype;
```
以上存在问题，构造函数中的属性和方法拿不到，原型对象中的方法和属性拿得到
**原型链的继承 主要分为3个步骤**
修改原型的指向 student的原型对象指向person原型对象
1.构造出2个构造函数
2.父类实例化一个对象，将子类的prototype指向父类实例化出来的对象
3.修改子类的constructor,指向自己的构造函数
如下
``` 
 function Person() {
        this.name = "廖科";
        this.pets = ["小煤球"];
    }
    
    //student应该继承person
      function Students() {
          this.num = "it like";
      }

```
第二步:
``` 

    //构造父类的实例，并且设置为子类的原型对象
    var p = new Person();
    //2.设置子类的原型对象为实例出来的Person对象
    Students.prototype = p;
```
第三步：
``` 
 //修复constructor的指向即可
    Students.prototype.constructor = Students;
```
问题：继承过来的属性，如果是引用类型则会被多个实例共享
当访问子类的时候，便可对应输出。
```
  var stu1 = new Students();
    console.log(stu1);
console.log(stu1.__proto__.constructor.name); 
```
注意，若是第三步没有修复的话，则会沿着指针链一直网上找。调用__constructor.name的时候，则会输出Person
以下方法:
``` 
 Students.prototype = Person.prototype;
    var stu = new Students();
    console.log(stu);
```
原型继承是修改了。但是无法获取person上的属性,name和pets无法获得。
链接 [__proto__和prototype的区别](https://www.jianshu.com/p/7d58f8f45557)
### 视频38重点
默认每个函数都有一个prototype属性，默认指向一个object对象。即为原型对象。可以增加属性和方法。
原型对象中有constructor,指向了构造函数。
原型对象中一般添加方法，实例出来的属性都可以拥有
call和apply方法。bind方法，以上都是function原型对象上的方法，
call和apply都是借方法来实现自己的东西，call(方法真实对象，参数1，参数2) apply([参数1，参数2])
如下:
```
  var demo1 = {
        "name":"ckq",
        log:function (param1) {
            console.log("我是个老实人" + param1);
        }
    };
    //call和apply就是借助其他对象的方法 自己没有的 去借别人的
    var demo2 = {
        "name":"cpp"
    };
    //demo2去借用demo1的方法 后面传递参数
    demo1.log.call(demo2,"ccc");
    //apply方法 参数以数组形式传进来
    demo1.log.apply(demo2,["ccc"]);``` 
```
demo2通过借调方法拿到了demo1中的方法
通过call和apply可以实现继承
``` 
  //继承里面也可以使用
    function Person() {
        this.name = "张三";
        this.age = "18";
    }
    function Student() {
        Person.call(this);//进行借调 student里面借调了person中的属性
        this.sex = "男";
    }
    console.log(new Student());
```
在student的构造函数中，通过call继承拿到了person中的属性，name和age 这样在new student的时候便会输出name,age和sex.
### 视频39重点 
借助构造函数继承，在构造函数内部进行原型的继承，否则在原型链上的继承会对引用数据类型产生错误，从而产生第一个对象对引用
类型的数据改变，第二个会随之改变。
``` 
 //继承里面也可以使用
    function Person() {
        this.name = "张三";
        this.age = "18";
    }
    function Student() {
        Person.call(this);//进行借调 student里面借调了person中的属性
        this.sex = "男";
    }
    console.log(new Student());
```
以上便是采用构造继承的方式。
### 视频40重点 寄生式组合继承
``` 
 //继承里面也可以使用
    function Person(name,age) {
        this.name = name;
        this.age = age;
    }
    function Student(sex,name,age) {
        Person.call(this,name,age);//进行借调 student里面借调了person中的属性 //上方采用组合继承
        //注意借调父函数的操作一定要放在最前面
        this.sex = sex;
    }


    //1.寄生式继承
  //手法是产生寄生式构造函数
 function Temp(){
        
 }
 //寄生组合式继承
 Temp.prototype = Person.prototype;
  //实例化对象
 var stuPrototype = new Temp();
 Student.prototype = stuPrototype; //这里是采用寄生式继承
 //修改constructor
 Student.prototype.constructor = Student;
 

    console.log(new Student("男","ckq","18"));
 console.log(new Student("女","cq","17"));

```
**可以用void 0来代替undefined 因为undefined在js中不是关键字，避免被篡改** 
**utf8编码 用在网页上统一显示中文等东方字体。js中的字符串是utf16编码**
js中的number类型 2^64-2^53+3
非整数的number类型无法用==(===)也不行
因为浮点数运算的精度导致等式左右两边不是严格相等的
所以比较方法应该用js提供的最小精度值
``` 
console.log(Math.abs(0.1 + 0.2 - 0.3) < Number.EPSILON);
```
symbol是es6引入的新类型，表示一切非字符串的对象的key的集合
symbol可以具有字符串类型的描述，但是即使描述相同，symbol也不相等
装箱转换 将string，number等转换成对应对象String,Number
拆箱转换，将String,Number转换成基本数据类型。采用valueOf或者toString方法
valueOf方法，返回给定参数的原生Number对象值。参数可以是原生数据类型，也可以是String类型。
toString方法，将指定类型或者对象转换成string字符串
valueOf和toString方法区别
1.valueOf应用于运算，toString应用于先死
对象转换成字符串alert(test)等优先调用toString,如果没有toString方法，就按照Object的toString 方法输出.
在进行强制字符串类型转换时优先调用toString方法，强制转化为数字优先调用valueof
在有运算操作符的情况下，valueof优先级高于toString
toString() 方法返回一个表示该对象的字符串。 
valueOf() 方法返回指定对象的原始值 
[链接1](https://blog.csdn.net/x_jagger/article/details/73430959)
[链接2](https://www.cnblogs.com/huaan011/p/6634349.html)
js垃圾回收机制:
看到一个小知识。IE的垃圾回收器是根据内存分配量搞的，变量的数量有个阈值，达到这个阈值，才会运行垃圾回收机制。但是遇到一种情况，就比如说一个程序本身自带的变量就超过了这个阈值，那垃圾回收机制，就会频繁运行。性能就下降了。
IE7之后，他们对垃圾回收器的机制做了调整，讲阈值变为动态，如果垃圾回收量低于15%，就将阈值加倍，如果回收了85%，就将阈值回复到默认值
### 视频42重点 内置对象扩展方法
扩充内置对象的方法
直接扩充Array上面的方法，不能达到继承的效果，而且还有可能覆盖。所以不行。
通过Array new出来一个新实例，随后将自己的function指向其。所以既可以有自己的方法，也可以用原来原型链上的方法。
<img src = "42.jpg" width = "50%" height = "50%"> 
``` 

    function MyArray() {

    };
    MyArray.prototype = new Array();
    MyArray.prototype.run = function () {
        console.log("跑");
    }
    var arr3 = new MyArray();
    console.log(arr3);
```
### 视频43重点 对象拷贝 浅拷贝
栈区和堆区的区别 栈区里面的都是值拷贝
堆区里面的都是地址拷贝
a = 20
b = a 值拷贝
浅拷贝只是拷贝地址 所以改变堆区里的东西会发生影响
``` 
 var obj = {name:"廖科",age:18};
    var copy = {};;
    //进行浅拷贝即可
    for(var key in obj){
        copy[key] = obj[key];
    }
    obj.name = "cc";
    console.log(obj);
    console.log(copy);

    //利用object.assign进行浅拷贝
    Object.assign(copy,obj,{address:"沙沟湖"});
    console.log(copy);
```
可以用for in或者assign函数进行拷贝
``` 
console.log(obj);
console.log(copy);
copy.pets.push(22);
    console.log(obj);
    console.log(copy);
```
改变copy的也会改变obj内的东西
### 视频44重点 实现深拷贝
``` 
 var obj = {
        "name": "ckq",
        "age": 18,
        "friend": ["a", "b"]
    };
    //实现深拷贝
    function deepCopyNewObj(fromObj,toObj) {
     //在函数中检查第一个对象是否有值，如果没有的话就初始化一个空的对象
     //如果有值的话就进行遍历 检查数据是什么类型 如果是基本的就进行值拷贝
        //如果是复杂的 就产生一个新的对象或数组进行深拷贝
        for(var key in fromObj){
            var fromValue = fromObj[key];
            if(!isObject(fromValue)){
                toObj[key] = fromValue;
            }else{
                //复杂数据类型
                //如果是引用数据类型的话 就再去调用一次这个方法 去内部拷贝这个对象的所有属性
                var tempObj = new fromValue.constructor; //拿到构造函数产生
                //自己还要进行递归操作，内部还有东西 tempObj原先是空的，随后不断的调用自己 将值拷贝进这个空的对象中

                deepCopyNewObj(fromValue,tempObj);
                toObj[key] = tempObj;
            }
        }
    }
    //判断类型
    function isObject(obj) {
        return obj instanceof Object;
    }
    function isArray(obj) {
        return Array.isArray(obj); //这种方式更好
        return Object.prototype.toString.call(obj) == '[object Array]'
    }
```
1.实现深拷贝的一个步骤，构造一个函数，
2.遍历对象，当对象的值时基本类型的时候，则进行值拷贝
3.当发现是引用树类型的时候，拿到改指定的构造函数产生一个新对象，是数组或者是对象，随后递归调用自己，将内部的值进行拷贝。
随后当递归结束后，将结束后的值进行值拷贝即可。
核心代码
``` 
  //复杂数据类型
                //如果是引用数据类型的话 就再去调用一次这个方法 去内部拷贝这个对象的所有属性
                var tempObj = new fromValue.constructor; //拿到构造函数产生 __proto__是隐式的 所以可以不显示调用
                //自己还要进行递归操作，内部还有东西 tempObj原先是空的，随后不断的调用自己 将值拷贝进这个空的对象中

                deepCopyNewObj(fromValue,tempObj);
                toObj[key] = tempObj;
```
进行验证
``` 
   //相当于在堆中新开辟了一个空间，由不同指针指向
    deepCopyNewObj(obj,temp);
    temp.friend.push("c");
    console.log(temp);
    console.log(obj);
```
temp中push一个新的不影响obj中的。
表明深拷贝成功。
### 视频45重点 私有变量和函数
私有：是一种访问权限，这种权限，是一个对象自己特有的，任何其他对象无法访问，外界也无法访问。
js中没有类的继承，实现私有 核心：唯一有作用域的是函数，函数特性来实现私有，
特权方法：能访问到私有变量和函数的方法称之为**特权方法**。
在函数体内部创建，用var来表示，是函数体内部私有的，外部访问不到
``` 
  function Calculator() {
        //私有变量
        var result = 0;
        //私有函数
        function checkNum(num) {
            var isNum = typeof num === 'number';
            if(!isNum) {
                alert("不是数字");
                return;
            }
         return isNum;
        }
        this.add = function (num) {
            result += num;
            //方便链式调用 将this返回回去
            return this;
        }
       this.logresult = function () {
           console.log(result);
           return this;
       }
    }
    var c = new Calculator();
    c.add(2).logresult();
    c.result = 100;//可以拿到并且修改
    console.log(c.result);
```
### 视频52 验证js是单线程的
浏览器的运行现在是多线程的，一个操作系统内部有多个进程，随后进程内部有多个线程，线程有时共享进程的一部分资源。
js是单线程运行的，可以用定时器的构造函数进行验证。
``` 
<button id = "btn">点我</button>
<script>
    var btn = document.getElementById("btn");
    var timeId = null;
    btn.onclick = function (ev) {
        //定时器操作
        //清除定时器
        clearTimeout(timeId);
      //2获取当前时间
        var cTime = Date.now();
        console.log("之前");
        //3.开启定时器 200毫秒之后执行
        setTimeout(function () {
            console.log(Date.now() - cTime, "ms后执行");
        //在一定范围内最接近的值去执行
            },200);
        console.log("之后");
    //4 耗时的任务
        for(var i = 0; i < 999999;i++){

        }

    }
</script>
```
为button添加一个点击事件，随后设置定时器的id，进来时第一步首先要清除定时器。随后设置延时器，延时200毫秒后执行，
可以看出打印的结果为，**之前，之后，随后再是204ms后执行**,
所以，js是单线程执行的，他看到一个setTimeout的操作，如果是多线程的话，会再开一个线程给他操作，然而是先打印之前，
之后，再去执行定时器，如果for循环比较多的话，则会延时的更加厉害，所以说js是单线程的。setTimeout这种都是回调代码，必须等
初始化代码执行完再去执行回调代码。
### 视频53重点 js为什么使用单线程
js使用单线程的原因，因为js是浏览器的脚本语言，主要作用是与用户交互，操作DOM,
如果js是多线程的话，会产生许多莫名其妙的错误。
### 视频54重点 事件循环模型
浏览器内核组成：1.html css文件解析模块 负责页面文本解析
              2.dom css模块 负责dom/css在内存中的相关处理
              3.布局和渲染模块 负责页面的布局和效果绘制
              4.定时器模块 负责定时器的管理
              5.网络需求模块 负责服务端请求(ajax/常规)
              6.事件响应模块 负责事件的管理
通过内核调用一个一个的模块，处理js的请求。
js的事件循环模型：
<img src = "./images/54.jpg" width = "50%" height = "50%">
浏览器中可分为多个模块 一个是js引擎模块，他是单线程执行的，二是浏览器api模块，他是多线程执行的，三是循环队列。
浏览器先利用js引擎初始化代码，将回调函数放到循环队列中，随后将dom的一些操作有浏览器api相结合。随后当浏览器点击等产生回调函数后，将回调函数放到循环队列中，当初始化代码都执行完毕了，再根据优先级，去执行循环队列中的回调函数。
#### 模块的组成：
事件管理模块和回调模块
#### 代码分类：
1.初始化执行代码(同步代码) 包含绑定DOM事件的监听，设置定时器，发送ajax请求等。
2.回调执行代(异步代码) 处理回调的逻辑。
#### 模型的运转流程
1.执行初始化代码 将事件回调函数交给对应模块处理
2.当事件发生时，管理模块会将回调函数及其数据添加到回调队列中。
3.只有当初始化代码执行完毕后，才会遍历读取回调队列中的回调函数执行。
### 视频55重点 设计模式
什么叫设计模式：是为了解决在开发中可能遇到的一些需求，而提出的一套解决方法。
设计模式的本质目的是为了项目变化造成的影响。
设计模式目的：1.高内聚低耦合，2.提高重用性，减少冗余代码。3.扩展性 4.稳定性
工厂设计模式：简单工程模式：给定属性和材料，进行批量生产。
工厂模式应用场景：不关心创建过程，创建过程比较少，依赖具体环境创建不同实例，
存储token:根据浏览器产生不同的存储对象。
### 视频56重点 复杂工厂模式
可以生产多种不同的产品 产品一样 但是属性不一样 生产线 产果汁 产香蕉汁等
我们生产一个构造函数fruitMaker是一个创建工厂。fruitMaker中扩展方法extend在这其中可以加入苹果汁和梨子汁的构造方法。
``` 
  //具备生产线 扩展生产能力
    FruitMaker.prototype.extend = function (obj) {
        for(var key in obj){
            //产生类似方法
            this[key] = obj[key];
        }
    }
```
将obj遍历 将obj中的赋值到this中
随后调用，进行造果汁
``` 
 //造苹果汁
    FruitMaker.prototype.extend({
        'Apple':function (meta) {
            console.log("造了一个苹果汁",meta);
        },
        //随后这个对象会分配给内部的key this[key] = obj[key];
        'Pear':function (meta) {
            console.log("造了一个梨子汁",meta);
        }
    })
```
随后进行生产，当传入一个时，比如传入Apple，要判断该工厂是否具有生产能力，具有生产能力后才进行生产。
``` 
//果汁工厂
    function FruitMaker() {
        //具备生产苹果的能力

    }
    FruitMaker.prototype.make = function (type,meta) {
        //进行生产 判断工厂是否具备生产能力
        if(typeof this[type] === 'function') {
            //判断当前这个type是具有构造的function 如果有说明有生产线
            var func = this[type];
            func.prototype = FruitMaker.prototype;
            return new func(meta); //返回生产线
        }else{
            //代表工厂中没有生产线
            throw '工厂不能生产这个产品';
        }


    }
```
只需要判断typeof this[type]是否是个function,如果是一个函数的话说明具有生产线。则可以进行生产，否则就抛出异常，另外构造的生产线的func需要改变原型指针指引，指向该工厂FruitMaker.
这样既可。
进行调用。
``` 
 //实例化水果工厂
    var fruitMaker = new FruitMaker();
    var appleObj = fruitMaker.make('Apple','一个苹果，一斤水');
     console.log(appleObj.constructor.name); //这里输出的是apple() 所以工厂应该都是同一个 里面的车间可以不相同 暴露给外部的必须都相同

```
可以看出打印出配料一个苹果，一斤水和constructor.name是FruitMaker,是水果工厂的构造器。
### 视频58重点 单例设计模式
概念：在整个程序的运行过程当中，
**一个类型只有一个实例对象** 全局中
通过指定的构造函数，无论创建多少次对象，都只有一个
全局用户信息，登录/注册 只有一个
js实现单例设计模式：1.全局变量 2.静态属性 3.闭包-惰性函数
老的方法
``` 
 function Tool() {

    }
    var t1 = new Tool();
    var t2 = new Tool();
    var t3 = new Tool();
    var t4 = new Tool();
    console.log(t1 === t2);
    //单例设计模式 无论实例化出来多少对象 其内存空间都是一样的
    //t1 = t2 = t3 = t4


```
实例构造出来的t1，t2,t3,t4在内存中占得地址都不相同，因此不是单例模式。
应用场景：全局只有一个
利用全局变量
在构造函数中进行判断，如果构造函数中没有，则创建，否则返回该全局变量
``` 
  //全局变量的单例模式
    var instance = null;
    function Tool() {
        //1.判断instance是否有值
        if(instance)
            return instance;
        //2.处理指向 指向当前创建的
        instance = this;
        this.name = "廖科";
        this.age = 18;
    }
    //进行实例化
    var t1 = new Tool();
    var t2 = new Tool();
    console.log(t1 === t2);

```
缺点：使用全局变量来保存单例实例，该全局变量可能在作用域中修改，或可能被覆盖
修改之后，创建出来的实例对象就不是原来的实例对象了。
### 视频59重点 即时闭包函数
2.即时构造函数，将instance隐藏进window中，这样可以避免外界访问从而修改。
``` 
  (function (w) {
        var instance = null;
        function Tool() {
            if(instance)
                return instance;
            instance = this;
            this.age = 18;
            this.name = "ckq";
        }
        w.Tool = Tool;//把tool构造函数作为window的全局变量来引用着
    })(window);
    var t1 = new Tool();
    var t2 = new Tool();
    console.log(t1 === t2);
```
将全局变量window传进，可以避免外部对instance的污染。保证单例。
