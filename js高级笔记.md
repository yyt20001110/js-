# js高级day1

## 一、作用域（scope）

​	变量与函数的可访问范围，控制着变量与函数的可见性与生命周期

### 		1、全局作用域

​		直接写在script下面的代码（或者单独的js文件）

​		可见性：在代码的任何地方都可以访问

​		生命周期：伴随着页面的生命周期（关闭页面时结束）

​		其他情况：函数内部不使用任何关键子声明的变量，**会成为全局变量**

### 		2、局部作用域

​		ES5之前，函数内部声明的变量，只能在函数内部访问，外部无法访问

​		可见性：函数外部无法访问

​		**生命周期：变量再函数调用完后就销毁**

```js
function fn() {
            const a = 1
            let b = 2
            var c = 3
        }
        fn()
        console.log(a,b,c)//不能访问,a,b,c是函数内部声明的局部变量
```

​		其他：

​			①使用let const 声明的变量，再{}中会产生块级作用域

​			② 块级作用域外部不能访问内部变量

​			③两个块级作用域中的变量互不影响

<hr>

## 二、作用域链

​	优先再当前作用域中查找变量；如果再当前作用域中找不到这个变量，则依次逐级查找父级作用域，直到全局作用域

<hr>

## 三、垃圾回收

​	内存不使用的时候，会被垃圾回收程序自动回收（js中内存的分配和资源的回收都是自动完成的）

<hr>

## 四、内存泄漏

### 	1、什么是内存泄漏?

​		不再用到的内存，没有及时释放，就叫做内存泄漏

### 	2.内存的生命周期是什么样的?

​		内存分配、内存使用、内存回收
​		全局变量-般不会回收; -般情况下局部变量的值,不用了，会被自动
​		回收掉

<hr>

## 五、释放内存

### 		1、引用计数法

​		在对象中添加一个引用计数器，每当有一个地方引用它时，计数器的值就加一；当引用失效时，计数器的值就减一。当某一时刻计数器的值为零时，这个对象就不再被使用。

​		引用计数法的优势是**简单、效率高**。缺点也很明显**单纯的引用计数很难解决对象之间的循环引用问题**

**算法:**
	①跟踪记录每个值被引用的次数。
	②如果这个值的被引用了一 次，那么就记录次数1

​	③多次引用会累加。

​	④.如果减少一个引用就减1。
​	⑤如果引用次数是0，则释放内存。

### 	2、标记清除法

​		第一步：标记。标记出需要回收的对象。

​		第二步：清除。清除被标记的对象。（也可以反过来，标记存活的对象，清除没有被标记的对象）

<hr>

## ==六、闭包 (需要理解+背诵)==

### 	1、内部函数引用外部函数变量的集合

​		闭包 = 内层函数+外层函数变量
​		产生条件：①有内层函数②内层函数使用了外层函数的变量。**内层函数一般作为返回值返回**

### 	2、闭包的作用

​		外部可以访问函数内部的变量但是不能修改这个变量

### 	3、闭包的应用

​		实现数据的私有化

​		可以做一些性能优化：

```js
// 防抖和节流 
// 防抖 debounce
// 节流 throttle 
```

### 	4、闭包的生命周期：

​		内部函数被创建时就产生闭包，但外部函数调用时才能执行函数定义，所以在外部函数被调用时产生，外部函数每次调用都会产生一个全新的闭包
毁（内部函数被垃圾回收了，闭包才会消失）

### 	5、相比于类（私有化属性方面）

```js
相较于类来说，闭包比较浪费内存空间（类可以使用原型而闭包不能），
需要执行次数较少时，使用闭包
需要大量创建实例时，使用类
```

### 	6、闭包的缺陷

​		存在内存泄漏问题，关掉浏览器之后 内存才会被回收。

<hr>

## 六、函数参数

### 		1、动态参数:arguments

​		内置对象，伪数组，接收实参

### 		2、rest剩余参数

​	（也就是参数是。。。arr的情况下）：是真数组	,有其他参数也有rest剩余参数的时候，剩余参数必须写在最后面

<hr>

## 七、展开运算符	

### 	1、展开运算符也就是。。。arr ,相当于是rest剩余参数的逆运算

```js
const arr = [1,2,3,4,5]
        console.log(Math.max(...arr))
        const arr1 = [7,8,9]
        console.log(arr3 = [...arr ,...arr1])
```

###     2、展开运算符也可以将伪数组转化为真数组

```js
 const divs = document.querySelectorAll('div')
        console.log([...divs])
```

### 	3、Array.from(伪数组) 也可以将伪数组转化为真数组

```js
 const arr = Array.from(divs)
        console.log(arr)
```

<hr>

## 八、this指向

​	this是一个变量，它的值是存的是一个对象

​	粗略的规则：谁调用 this就指向谁

### 		1、在全局坏境中，this 指向的是widow，

### 	2、构造函数的this 指向实例对象

### 	3、call() 、apply() 、bind() 的this指向指定的	

```js
 fn.call(window)//指定this指向
    fn.apply(obj) //指定this指向   
    fn.apply() //this指向函数本身  
    fn.call() //this指向函数本身
// 2.两者区别
    //   传参方式不同
    fn.call(obj,实参)  //第一个参数指定this指向，第二个参数指定实参的值
    fn.apply(obj,[实参])   
```

```js
// 1.bind()用来创建一个新的函数,第一个参数是绑定死this，后面的参数是绑定死实参，只对于生成的新函数
// 2.
        const obj = {
            name:1
        }
        
        function fn(a,b){
            console.log('fn执行',this,a,b)
        }
        const newfn = fn.bind(obj,10) //绑定死newfn的this 和newfn()第一个参数为10
        newfn(1,2)// 10 1
```



### 		4、函数内部，this指向取决于这个函数被调用的方式，

​		**①普通函数的调用**

​			this指向的是widow

```js
function fn（）{
	console.log(this)								
}
fn()//这里的fn()相当于widow.fn()
//所以这里this指向的是widow
```

​		**②函数作为对象的方法调用**

​			this指向这个对象	

```js
let obj = {
            name:'张三',
            fn1:function(){
                console.log(this)
            }
        }
        obj.fn1()//this 指向obj
        
```

​		**③事件绑定中，this指向注册（绑定）事件的元素**	

```js
// 事件绑定中，this指向注册（绑定）事件的元素
        const btn  = document.querySelector('button')
        btn.addEventListener('click',function(){
            console.log(this)//this 指向的button
            this.innerHTML = '张三'
        })
```

<hr>

## 九、检测数组的方式

​	1、arr instansof   Array，返回true或false

​	2、Objeect.prototype.toString.call(arr)，返回的是Arrary

​	3、Array.isArray（arr）：返回true或false

```js
const arr = [1,2]
        console.log(arr instanceof Array)
        console.log( Object.prototype.toString.call(arr))
        console.log(Array.isArray(arr))
```

<hr>

## 十、解构赋值(模式匹配)

​	ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。

### 	1、数组解构

​		[a,b,c] = [1,2,3] 得到a =1,b=2,c=3

​		①数组的解构可以用于交换变量

```js
;[a,b] = [b ,a]//中括号前面加括号
```

​		②解构赋值可以给左边的变量设置初始值

​		③当右边对应的位置不存在元素，或者严格等于undefined使用默认值

```js
const [a = 1, b = 1] = ['undefined', undefined]
        console.log(a, b)// undefined 1
		//当右边对应的位置不存在元素，或者严格等于undefined使用默认值
```

​		④当右边不是数组时，就会报错

### 	2、对象解构

```js
const {name, age}  = {name:'练练', age:18}//可以交换顺序，因为对象是无序的
//改变属性名字：
const { name: user, age } = obj
        console.log(user, age)
```

<hr>




# js高级day2

---



## 一、深入对象

### 1.创建函数的三种方式

#### 1.1 new函数声明

```js
new Object({ name: 'tom' })
```

#### 1.2 字面量声明

```js
const o = { name: 'tom' }
```

#### 1.3 构造函数

```js
//构造函数
 function Person(name){
            this.name = name
 }
```



---

### 2.构造函数

```js
 // 1. 构造函数， 相当于一个模板， 可以创建一系列具有相同属性和方法的对象。
 // 2. 通过构造函数创建对象的过程叫做实例化  ==> new 一个对象的过程，
 // 3. 创建好的这个对象， 也叫做实例 （实际的例子，具体的某一个）

        function Person(name,age,sex){
            this.name = name
            this.age = age
            this.sex = sex 
            this.sayHi = function(msg){
                // console.log('hi')
                console.log(msg)
            }
            //return 对象
        }
        const zjl = new Person('周杰伦',18,'男')
        console.log(zjl)
        const ll = new Person('萝莉',18,'女')
        console.log(ll)
        zjl.sayHi('我测你们码')
```



#### 2.1  约定

- 首字母要大写

- 需要用new实例化

  

#### 2.2 实例化过程

1. 创建新空对象
2. 构造函数this指向新对象
3. 执行构造函数代码
4. 返回新对象  (构造函数里面不需要写return)

---

### 3.实例成员&静态成员

#### 3.1实例成员

- 实例对象中的属性和方法

- 通过实例对象去访问

  ```js
  const p1 = new Person('练练', 18, '男')
  ```

  p1为构造函数创建出来的实例对象

#### 3.2静态成员

- 构造函数上的属性和方法

- 与实例对象有关

  

---

## 二、内置构造函数

### 思想：一切皆对象

- JS中几乎所有的数据都可以基于构成函数创建，不同的构造器创建出来的数据拥有不同的属性和方法

- 引用类型：Object，Array，RegExp，Date 等

- 包装类型：String，Number，Boolean 等

  

---

### 1.Object

#### 1.1  Object.values()

返回由传入对象的属性名组成的字符串数组

```js
const obj = {name:'练练', age:19}
console.log(Object.values(obj))
```

#### 1.2  Object.keys()

返回由传入对象的属性名组成的字符串数组

```js
const obj = {name:'练练', age:19}
console.log(Object.keys(obj))
```

#### 1.3   Object.assign()

拷贝

```js
 const o1 = { a: 1 };
        const o2 = { b: 2 };
        const o3 = { c: 3 };
        const obj2 = Object.assign(o1, o2, o3);
        console.log(obj2); // { a: 1, b: 2, c: 3 }
        console.log(o1);  // { a: 1, b: 2, c: 3 }
        console.log(obj2 === o1) //true
```

---

### 2.Array

#### 2.1 forEach

```js
const arr = [1, 2, 3, 4, 5];
    arr.forEach(function (item,index) {
    console.log(item);
    console.log(index)                  
})
```



#### 2.2 filter

```js
 const arr = [10, 20, 30, 40]
        const res = arr.filter(function(item) {
            // return 
            // console.log(item)
            // console.log(index)
            return item >= 20
        })
        console.log(res)
```



#### 2.3 map



#### 2.4 reduce

语法：arr.reduce(function(){}, initValue)

```js
 const arr = [1, 2, 3]
   const res = arr.reduce((prev, cur) => {
    console.log(prev)
    return prev + cur
   }, 0)
   // 注意：1. 如果有初始值，prev第一次调用时，就是写的初始值
   //      2.如果不写初始值，initValue，prev为数组的第一个元素 
   console.log(res)
   //reduce 常用于数组的求和
   const resArr = arr.reduce((pre,cur) => pre + cur , 10)
   console.log(resArr)
```

## 三、原型的五条规则

1. 所有的引用类型（数组，对象，函数），都具有对象的特性，可以自由扩展属性

   ```js
   const obj = {} //obj.a = 100
   const arr = [] //arr.a = 100
   function fn(){}//fn.a = 100
   ```

2. 所有的对象，都有一个__proto__属性，属性值是一个普通的对象  ？原型对象（__proto__==>[[prototype]]）

   ```js
   console.log(obj.__proto__)
   console.log(arr.__proto__)
   console.log(fn.__proto__)
   // __proto__也叫做隐式原型
   ```

   

3. 所有的函数，都有一个prototype属性，属性值也是一个普通的对象  ==>原型对象

   ```js
   console.log(fn.prototype)  //prototype 显示原型 
   ```

4. 所有对象的隐式原型 (__proto__) , 指向它的构造函数的显示原型(prototype)  指向==>等于

   ```js
   function Person(name , age){
       this.name = name
       this.age = age
   }
   const p =new Person('ll',18)
   console.log(p.__proto__===Person.prototype)
   ```

5. 当试图得到一个对象的某个属性的时，如果这个对象本身没有这个属性，那么会去它的__proto__中寻找
