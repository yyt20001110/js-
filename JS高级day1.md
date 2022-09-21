# JS高级day1

## 1.作用域

### ==全局作用域==

尽量少使用，避免变量污染

直接写在script中的代码（或者单独的js文件）

可见性：在全局作用域中声明的变量，在代码的任何地方都可以访问

生命周期：伴随着页面的生命周期（关闭页面的时候结束）

```js
//1.函数内部可以访问全局作用域中的变量 
const num = 10
        function fn() {
            //函数内部可以访问
            console.log(num)
        }
        fn()
```

```js
//2.函数内部不适用任何关键字声明的变量，会成为全局变量
        function bar() {
            a = 20
        }
        bar() //需要调用一下
        console.log(a)
```



### ==局部作用域==

ES5之前,局部作用域就是函数作用域

在函数内部声明的变量只能在函数内部访问，外部无法访问

可见性：函数外部无法访问局部作用域

生命周期：变量在函数调用执行完后，就销毁（清空）

1. 函数作用域

   ```js
    function getSum() {
               const b = 10
           }
           getSum()
           console.log(b) //会报错 函数外部无法访问内部的变量
   ```

   

2. 块级作用域

   ```js
   //1.用var声明的变量会提升到当前作用域的最顶层，声明提升了，但是赋值没有提升
   //2.用var声明的变量如果不复制，默认就是undefined
           var tmp = new Date()
           function fn() {
               console.log(tmp)
               if (false) { //if是语句
                   var tmp = "hello word"
               }
           }
           fn()
           //===>变量提升变成下面
           function fn() {
               var tmp
               console.log(tmp)
               if (false) {
                   tmp = "hello word"
               }
           }
           // 块级作用域
           //使用let或者const声明的变量，在{}中会产生块级作用域
           var tmp = new Date() //全局变量
           function fn() {
               console.log(tmp)
               if (false) {
                   let tmp = "hello word"
               }
           }
           fn()
   //1.let const 声明的变量 不会有变量提升
   //2.块级作用域外部不能访问内部的变量
   //3.两个块级作用域中的变量相互不影响
           for (let i = 0; i <= 3; i++) {
               console.log(i)
           }
           for (let i = 0; i <= 3; i++) {
               console.log(i)
           }
           // console.log(i)
   ```

   

### ==作用域链==

作用域链  本质是底层变量的查找机制

查找规则：

1.优先在当前作用域中查找变量

2.如果当前作用域中找不到这个变量，则依次逐级查找

```js
	//全局作用域
        let a = 2
        let b = 1

        function f() {
            //let a = 6
            function g() {
                //b=3
                console.log(b)
            }
            console.log(a)
            g()
        }
        f()
```

### ==垃圾回收机制==

 Garbage Collection 简写GC

内容在不使用的时候，会被垃圾回收程序自动回收 （js中内存的分配和资源的回收都是自动完成的）

- ####  	引用计数法

​     1. 跟踪记录每个值（变量）被引用的次数，

​     2. 如果这个变量被引用了一次， 那么就记录次数为1

​     3. 多次引用会累加

​     4. 如果减少一个引用就减1

如果引用次数为0， 则释放内存

```js
 function problem() {
            // {} ==> new Object()
            let oA = {}
            let oB = {}
            oA.a = oB // oB被oA引用
            oB.a = oA // oA被oB引用
        }

        problem()
        problem()
//循环引用的时候，内部创建的对象相互引用，计数永远不会为0
//这个对象会一直占用着内存空间，造成内存泄露

```

- #### 标记清除法

​    从全局作用域出发，如果找不到这个对象的引用，就认为它不再使用了，直接回收掉

### ==闭包==

闭包的定义，内部函数引用外部函数的变量的集合

闭包=内层函数+外层函数的变量

1.首先，要有内层函数

2.内层函数使用了外层函数的变量

```js
 function outer() {
            //外层函数outer  把内层函数和这个a变量包起来
            let a = 10
                //内层函数
            function fn() {
                console.log(a)
            }
            fn()
        }
        outer()
```

| 作用域     | scope   |
| ---------- | ------- |
| 局部作用域 | local   |
| 全局作用域 | global  |
| 块级作用域 | block   |
| 闭包       | closure |

常见的闭包形式

```js
    <script>
        // function outer(){
        //     let a=10
        // }
        // outer()
        // console.log(a)
        //1.外部作用域无法访问局部作用域中的变量

        //
        //2.闭包的作用：通过闭包，外部可以访问函数内部的变量
        // function outer() {
        //     let a = 10

        //     function fn() {
        //         console.log(a)
        //     }
        //     return fn
        // }
        // const fun = outer()
        // const fun=function(){
        //     console.log(a)
        // }
        // console.log(fun)
        // 我们在外部调用fun的时候，执行到console.log(a)，找的是outer内部的变量的a
        // fun()

        //3.简单的写法
        function outer() {
            let a = 100
            return function() {
                console.log(a)
            }
        }
        // outer()()
        const foo = outer()
        foo()
    </script>
```



## 2.箭头函数

```js
 // 表达式的方式
        const fn = function(){
            console.log(1)
        }

        // 箭头函数 =>  来定义一个函数
        const fn1 = () => {
            console.log(1)
        }
        fn1()

        // 1. 传参 
        const fn2 = (x) => {
            console.log(x)
        }
        fn2(8)

        // 2. 只有一个形参的时候，可以省略小括号 ()
        const fn3 = x => {
            console.log(x)
        }
        fn3(9)

        // 3. 只有一行代码的时候， 可以省略花括号 {} 大括号   ()小   []中（方）  {} 大（花） 
        const fn4 = x => console.log(x)
        fn4(6)

        // 4. 只有一行代码的时候， 可以省略return
        const fn5_ = x => {
            return x + x 
        }
        const fn5 = x => x + x 
        console.log(fn5(6))

        // 5. 箭头函数可以直接返回一个对象， 但必须在对象外面加上小括号
        const fn6_ = (name) => {
            return {user_name : name}
        }
        const fn6 = name => ({user_name : name})

        console.log(fn6('周杰伦'))

```

this指向：上一层

## 3.动态参数和剩余参数

1. 动态参数

   ```js
    //1.所有的函数都有一个内置的对象
           //2.动态参数arguments===>伪数组
           //3.arguments接收了传过来的所有参数
   
           function getSum() {
               //伪数组：有索引，有length，没有数组方法
               // console.log(arguments)
               let sum = 0
               for (let i = 0; i < arguments.length; i++) {
                   sum += arguments[i]
               }
               return sum
           }
   
           //getSum(1,2,3)
           const res = getSum(1, 2, 3, 4, 5, 6)
           console.log(res)
   ```

   

2. 剩余参数（rest）

```js
 //rest剩余参数
        //rest参数是一个真数组
        // function getSum(...arr) {
        //     let sum = 0
        //     for (let i = 0; i < arr.length; i++) {
        //         sum += arr[i]
        //     }
        //     return sum
        // }
        // const res = getSum(1, 2, 3)
        // console.log(res)



        const getSum = (...arr) => {
            let sum = 0
            for (let i = 0; i < arr.length; i++) {
                sum += arr[i]
            }
            return sum
        }
        const res = getSum(1, 2, 3)
        console.log(res)
```

## 4.解构

1. 数组结构

   ```js
    // 解构赋值的本质 就是一种模式匹配  等号左右两边长得一样
           let [a, [
               [b], c
           ]] = [1, [
               [2], 3
           ]]
           console.log(a, b, c)
   
           let [, , d] = ['1', '2', '3']
           console.log(d)
   
           let [head, ...tail] = [1, 2, 3, 4, 5]
           console.log(head, tail)
   
           let [fn = 1] = []
           console.log(fn)
   
           let [bar = 6, foo = 6] = [undefined, undefined]
           console.log(bar, foo)
   ```

   

2. 对象结构

   变量重命名

   ```js
    // 对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。
           // const obj = {
           //     name:'周杰伦',
           //     age: 20
           // }
   
           // // 对象的解构没有顺序影响
           // const {age, name} = obj
           // console.log(age, name)
   
           // const pig = [{
           //     uname: '佩奇',
           //     age: 6
           // }]
   
           // const [{
           //     uname: user_name,
           //     age
           // }] = pig
   
           // ==>
           // const uname = pig[0].uname
           // const age = pig[0].age
           // console.log(user_name, age)
   
   
   
           // 练习
           // const pig = {
           //     name: '佩奇',
           //     age: 6
           // }
           // const {
           //     name: uname,
           //     age
           // } = pig
           // console.log(uname, age)
   
           const goods = [{
               goodsName: '小米',
               price: 1999
           }]
           const [{
               goodsName,
               price
           }] = goods
           console.log(goodsName, price)
   ```

   多级解构

   ```js
    const pig = {
                   name: '佩奇',
                   family: {
                       mother: '猪妈妈',
                       father: '猪爸爸',
                       sister: '乔治'
                   },
                   age: 6
               }
               // const {name, family:{mother, father, sister}, age} = pig
               // console.log(name)
               // console.log(family)
               // console.log(age)
               // console.log(name, mother, father, sister, age)
   
           // 对象的解构还是一种模式匹配， 这里family相当于是找到pig里面的family
   
           const pig2 = [{
               name: '佩奇',
               family: {
                   mother: '猪妈妈',
                   father: '猪爸爸',
                   sister: '乔治'
               },
               age: 6
           }]
   
           const [{
               name,
               family: {
                   mother,
                   father,
                   sister
               },
               age
           }] = pig2
           console.log(name, mother, father, sister, age)
   ```

   

   函数参数的解构

   ```js
    // 1. 函数参数的解构
           const pos = {
               x: 10,
               y: 20
           }
   
           function move(pos) { // 形参
               console.log(pos)
                   // const x = pos.x
                   // const y = pos.y
               const {
                   x,
                   y
               } = pos
               console.log(x, y)
           }
           move(pos) // 实参
   
           // 2. 在形参的位置，直接解构
           function move({
               x,
               y
           }) { // 形参
               // let {x,y} = pos
               console.log(x, y)
           }
           move(pos) // 实参
   
           // 3. 在形参的位置，解构，还可以重命名
           function move({
               x: aa,
               y
           }) { // 形参
               // let {x,y} = pos
               console.log(aa, y)
           }
           move(pos) // 实参
   
           // 4. 如果函数的参数是数组的情况
           function add([x, y]) {
               // let [x, y] = [1, 2]
               return x + y
           }
   
           const res = add([1, 2])
           console.log(res)
   ```

   

## 5.array.filter()方法

```js
// array.filter()  filter 筛选 过滤的意思

        // 作用： 创建一个新的数组，新数组的元素 是通过筛选后的元素 （满足筛选的条件）
        // array.filter接收一个函数作为参数 ，这个函数一般要写return ， 返回true或者false

        // 这个函数又接收两个参数
        // 第一个参数 ==> item, 数组里面的每个元素
        // 第二个参数 ==> index, 索引号

        // 返回值： 由满足条件的元素， 组成的新数组。

        const arr = [10, 20, 30, 40]
        const res = arr.filter(function(item) {
            // return 
            // console.log(item)
            // console.log(index)
            return item >= 20
        })
        console.log(res)

        // 过滤
        const resArr = arr.filter(el => el > 20)
        console.log(resArr)

        // 把原来的每个元素，执行某个方法，得到的每个元素组成一个新的数组
        const resMap = arr.map(el => el + 6)
        console.log(resMap)
```

