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

#### Eg1小练习

```js
 const spec = { size: '40cm*40cm', color: '黑色' }
        // const pj = Object.values(spec).join('/')
        // document.documentElement.innerHTML = pj
        document.documentElement.innerHTML = Object.values(spec).join('/')
```

#### Eg2小练习

```js
const gift = '50g茶叶,清洗球'
        //1.把字符串以，分隔符号，转为数组
        const arr = gift.split(',')
        console.log(arr)
        //2.根据数组里面的元素 ，利用map转为[`<span></span>`,`<span></span>`] 这样数组
        const resArr = arr.map(el => `<span>【赠品】${el}</span><br>`)
        //3.将这个得到的数组再转换为字符串，放到div里面
        document.querySelector('div').innerHTML = resArr.join('')
```



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

```js
const array1 = [1, 4, 9, 16];

// pass a function to map
const map1 = array1.map(x => x * 2);

console.log(map1);
// expected output: Array [2, 8, 18, 32]
```



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

##### 2.4.1 涨薪小案例

```js
  const arr = [{
        name: '张三',
        salary: 10000
        }, {
        name: '李四',
        salary: 10000
        }, {
        name: '王五',
        salary: 20000
        }]
    
    // const money = arr.reduce(function(prev,cur){
    //     return prev + cur.salary*1.3
    // },0)
    const money =arr.reduce((prev,cur) => prev + cur.salary * 1.3,0)
    console.log(money)
```



#### 2.5 find

1.语法：arr.find(cbfn)

2.作用：返回数组中满足条件的第一个数组元素，返回undefined

3.参数 cbFn  ===>这个函数中，一般写上return

​      cbFn

​      第一个参数：item 当前元素

​      第二个参数: index 当前元素的索引

4.数组中第一个满足条件的元素 / undefined

5.注意：返回的是第一个满足条件的

```js
const arr = ['red','green','blue']

        const res = arr.find(function(item){
            return item === 'blue'
        })
        const resArrow = arr.find(el => el==='blue')
        console.log(res,resArrow)

        //实际使用，搜索查找
        //需求：找到数组中名字为小米的那个元素（对象）？
        const arrTemp = [{name:'小米',price:1999},{name:'华为',price:6999}]
        const mi = arrTemp.find(item => item.name === '小米')
        console.log(mi)
```

---

#### 2.6 findindex

1.语法:arr.findIndex(cbFn)

2.作用：查找数组中满足条件的第一个元素的索引号，如果没有，返回-1

3.参数：cbFn

cbFn的参数

第一个参数：item 当前元素      第二个参数：index 当前元素的索引号（可选）

返回值：索引号 / -1

```js
 const arr = [{name:'小米', price:1999},{name:'华为', price:5999},{name:'iPhone', price:8999}]

        const index = arr.findIndex(function(item){
            return item.name === 'iPhone'
        })
        const i = arr.findIndex(i => i.name==='iPhone')
        console.log(index,i)
```



---

#### 2.7 every

1.语法:arr.every(cbFn)

2.作用：检测数组内的所有元素是否都满足指定的条件，如果都满足，返回true / 否则 false

3.参数：cbFn  ===> return

​    cbFn的参数

​    第一个参数：item 当前元素

​    第二个参数：index 当前元素的索引号（可选）

4.返回值：boolean ==> true / false

```js
        const arr = [10,20,30]
        const res = arr.every(el => el >= 10)
        const res1 = arr.every(el => el >= 20)  //第一个10不满足
        console.log(res,res1)
```

---

#### 2.8 some

arr.some(cbFn) : 检测数组中的元素是否满足指定的条件，只要有一个满足，返回true / 否则 false

```js
const arr = [10,20,30]
        const resSome = arr.some(el => el >= 40)
        console.log(resSome)
```

#### 2.9 includes

arr.include() : 判断数组中是否有这个元素，有返回true / 否则 false

```js
const arr = [10,20,30]
        const resincludes = arr.includes(10)
        console.log(resincludes)
```



---

### 3.String

---

#### 3.1 split

str.split(分隔符作用：使用指定的分隔符，将字符串分隔，得到一个字符串数组

```js
const str = 'The quick brown fox jumps over the lazy dog.';
        const words = str.split(' ');
        console.log(words)

        const str1 = '2022-9-24'
        const arr = str1.split('-')
        console.log(arr)
```

---

#### 3.2 substring

1.语法：str.substring(indexStart[,indexEnd])

2.作用：从字符串中截取某一段出来

3.注意：如果省略indexEnd，取到最后

4.前闭后开区间[start,end) end的索引号 不包含在生成的字符串内

```js
 const str3 = '大家喜欢吃冰激凌吗 ？'
        console.log(str3.substring(0))
        console.log(str3.substring(2))
        console.log(str3.substring(4))
        console.log(str3.substring(2,4))
```

#### 3.3 startsWith

1.str.startsWith(searchString[, position])

2.作用： 判断是否以某个字符字串开头

3.searchString  : 要搜索的字符串

4.position    : 在str中开始搜索的位置 默认为0 （可选）

5.返回值： true / false

#### 3.4 includes

```js
  //判断一个字符串中是否包含另一个字符串  true / false
        const str = "mother fuck"
        console.log(str.includes('fuck'))  //true
        console.log(str.includes('f'))     //true
        console.log(str.includes('p'))     //false

        //includes(substr[,pos]) 也可以有pos位置
        console.log(str.includes('fuck',6))//true
```



---

### 4.Number

---

#### 4.1 toFixed

num.toFixed()

设置小数的保留位数

```js
const num = 10.923
        const res = num.toFixed(2)
        console.log(res)
        console.log(typeof res)

        //如果不传参数,四舍五入为整数形式的字符串
        const res2 = num.toFixed()
        console.log(res2)
```

---



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

## 四 、flex

flex  ==>  flex-grow; flex-shrink; flex-basis;  0 1 auto

### 1. 容器的属性（6个设置在容器上）

#### 1.1`flex-direction`

决定主轴的方向（即项目的排列方向）。

- `row`（默认值）：主轴为水平方向，起点在左端。
- `row-reverse`：主轴为水平方向，起点在右端。
- `column`：主轴为垂直方向，起点在上沿。
- `column-reverse`：主轴为垂直方向，起点在下沿。

```css
.box {
flex-direction: row | row-reverse | column | column-reverse;
}
```

#### 1.2 flex-wrap

决定是否换行

- `nowrap`（默认）：不换行。
- `wrap`：换行，第一行在上方。
- `wrap-reverse`：换行，第一行在下方。

```css
.box{
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```

#### 1.3 flex-flow

flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`。

```css
.box {
  flex-flow: <flex-direction> || <flex-wrap>;
}
```

#### 1.4  justify-content

定义了项目在主轴上的对齐方式。

- `flex-start`（默认值）：左对齐
- `flex-end`：右对齐
- `center`： 居中
- `space-between`：两端对齐，项目之间的间隔都相等。
- `space-around`：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

```css
.box {
  justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

#### 1.5 align-items

属性定义项目在交叉轴上如何对齐。

- `flex-start`：交叉轴的起点对齐。
- `flex-end`：交叉轴的终点对齐。
- `center`：交叉轴的中点对齐。
- `baseline`: 项目的第一行文字的基线对齐。
- `stretch`（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

```css
.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
```

#### 1.6 align-content

定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

- `flex-start`：与交叉轴的起点对齐。
- `flex-end`：与交叉轴的终点对齐。
- `center`：与交叉轴的中点对齐。
- `space-between`：与交叉轴两端对齐，轴线之间的间隔平均分布。
- `space-around`：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
- `stretch`（默认值）：轴线占满整个交叉轴。

```css
.box {
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

---

### 2.项目的属性

#### 2.1 order

定义项目的排列顺序。数值越小，排列越靠前，默认为0。

```css
.item {
  order: <integer>;
}
```

#### 2.2 flex-grow

定义项目的放大比例，默认为`0`，即如果存在剩余空间，也不放大。

> 规则：如果所有项目的`flex-grow`属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的`flex-grow`属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

```css
.item {
  flex-grow: <number>; /* default 0 */
}
```

#### 2.3 flex-shrink

定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。

> 如果所有项目的`flex-shrink`属性都为1，当空间不足时，都将等比例缩小。如果一个项目的`flex-shrink`属性为0，其他项目都为1，则空间不足时，前者不缩小。

```css
.item {
  flex-shrink: <number>; /* default 1 */
}
```

#### 2.4 flex-basis

> 在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为`auto`，即项目的本来大小。它可以设为跟`width`或`height`属性一样的值（比如350px），则项目将占据固定空间。
>
> 

```css
.item {
 flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```

#### 2.5 flex

> `flex`属性是`flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为`0 1 auto`。后两个属性可选。

> 该属性有两个快捷值：`auto` (`1 1 auto`) 和 none (`0 0 auto`)。

> 建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。

```css
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```

#### 2.6 align-self

> `align-self`属性允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`。

```css
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

---

# js高级day3

> 深入面向对象

## 一、编程思想

### 1.面向过程

面向过程就是分析出解决问题所需要的步骤，然后用函数一步一步的实现，使用的时候再一个一个的依次调用就可以了。

简述：面向过程，就是按照我们分析好了的步骤，按照步骤解决问题

### 2.面向对象

面向对象是把事务分解成一个个对象，然后由对象之间分工与合作。

简述：面向对象是以对象功能来划分问题，而不是步骤。

#### 2.1 封装性 

#### 2.2 继承性

#### 2.3 多态性

---



### 3.两者的优缺点

#### 3.1 面向过程编程

优点：性能比面向对象高，适合跟硬件联系很紧密的东西，例如单片机就采用的面向过程编程

缺点：没有面向对象易维护、易服用、易扩展

#### 3.2 面向对象编程

优点：易维护、易服用、易扩展，由于面向对象有封装、继承、多态性的特性，可以设计出低耦合的系统，使系统更加灵活、更加易于维护

缺点：性能比面向过程低

---

## 二、构造函数

> 构造函数里面的this指向new出来的实例

- 封装是面向对象思想中比较重要的一部分，js面向对象可以通过构造函数实现的封装。
- 同样的将变量和函数组合到了一起并能通过this实现数据的共享，所不同的是借助构造函数创建出来的实例对象之间是彼此不影响的

1. 构造函数体现了面向对象的封装特性
2. 构造函数创建的实例对象彼此独立，互不影响

### 1.内存浪费版本

```js
内存 function Star(uname, age) {
this.uname = uname
this.age = age
this.sing = function() {
console.log('我会唱歌')
					}
} 
const ldh = new Star('刘德华', 18)
const zxy = new Star('张学友', 19)
```

==方法虽好，但是会造成内存浪费==

### 2.挂载原型对象(省内存版)

```js
 // 1.公共的属性写到构造函数里面
        function Star(name,age){
            this.name = name
            this.age = age
            // this.sing = function(){
            //     console.log('唱歌')
            // }
        }

        //2.公共的方法写到原型对象上，节约了内存空间
        Star.prototype.sing = function(){
            console.log('唱歌')
        }
        
        const ldh = new Star('刘德华',18)
        const zjl = new Star('周杰伦',19)
        const ll = new Star('练练',15)
        ldh.sing()
        zjl.sing()
        console.log(ldh.sing === zjl.sing)  //true
```



## 三、 原型 （个人理解）

> 原型:原始的模型，原始祖先
>
> JS里面：原型就是一个对象，也叫做原型对象
>
> 原型方法中的this ，指向的也是new出来的实例

![06168fc392ac20525347e9dc1eb086d](../../Pictures/06168fc392ac20525347e9dc1eb086d.png)

### 1. prototype  

相当于指针，从构造函数指向构造函数的原型对象

(构造函数).prototype--->(构造函数的原型对象) 

```js
//所有通过构造函数创建的实例，都共享原型对象上的属性和方法
        function Star(name,age){
            this.name = name
            this.age = age
        }
```

```js
//公共的方法写到原型对象上
        Star.prototype.sing = function(){
            console.log('唱歌')
        }
```

```js
//原型上也可以添加属性
        Star.prototype.cheer = 'Every step count' //每天进步一点点
```

```js
const ldh = new Star('刘德华',18)
        const zxy = new Star('张学友',19)
        console.log(ldh.__proto__)
        console.log(zxy) 
```

---



### 2. constructor

原型对象的默认属性constructor(存在于原型对象上)，指向这个构造函数本身

```js
console.log(Person.prototype.constructor === Person)
//true
```

案例

```js
 function Star(name){
            this.name = name
        }
        const ldh = new Star('刘德华')
        console.log(ldh)
```

```js
constructor在哪里？
        // ==>1. ldh.__proto__ ==>找到原型
        // ==>2. Star.prototyoe ==>找到原型
```



### 3.    `__proto__`

所有的对象都有`__proto__`(隐式原型)，属性值是一个对象（原型对象）

所有的对象__proto__(隐式原型)指向它的构造函数的prototype(显示原型)

因为函数也是属于对象的，所以函数也有__proto__属性

```js
console.log(ldh.__proto__ === Star.prototype)
```

`__proto__`作用

它相当于是一个桥梁 实例通过 `__proto__` 就可以访问到它的原型对象

```js
//1.实例对象.__proto__ === 构造函数.prototype
        //2.构造函数.prototype.constructor === 构造函数
        Animal.prototype.constructor === Animal
        cat.__proto__.constructor === Animal
        //3.返回指定实例（对象）的原型（对象）
        //Object.getPrototypeOf(obj)
        console.log(Animal.prototype)
        console.log(cat.__proto__)
        console.log(Object.getPrototypeOf(cat))

        Object.getPrototypeOf(cat) === cat.__proto__   //true
        Object.getPrototypeOf(cat) === Animal.prototype  //true
```

### 4.原型链

```js
 //原型链:
        //1.Person.prototype也是一个对象，所以，它也有一个__proto__属性 （规则2）
        // 既然Person.prototype 它（person的原型）是一个对象（实例），那么，它的构造函数是谁呢？

        //Person.prototype 这个对象的构造函数 ===> Object
        //Person.prototype = new Object()
        //实例.__protp__ === 构造函数.prototype (构造函数)
        Person.prototype.__proto__ === Object.prototype

        //2.Object.prototype 得到的是什么 原型，谁的原型 Object构造函数的原型
        Person.prototype.cunstructor === Person
        //2.1  Object.prototype 原型，原型对象上默认有一个constructor，指向构造函数Object
        //2.2 Object这个构造函数
        // Object.prototype ==> 访问到它的原型
        // 2.3 
        // Person.prototype.__proto__ ==> 访问到的还是object的原型
       

       // 3. Object是构造函数，Object.prototype ==> 原型
        // Object.prototype ==> 原型对象，也是对象 
        // 规则2 ：所以Object.prototype.__proto__ 属性
        // 但是 ==> Object.prototype.__proto__ 指向 null
        Object.prototype.__proto__ === null

        // 正常原型链都会终止于Object的原型对象
        // Object.prototype ==> 它是原型
        // Object.prototype.__proto__ 访问它 Object.prototype的原型， null
        // Object原型的原型是null
```

```js
//原型链
        //每个对象通过__proto__属性都能访问到它的原型，原型也有它的原型
        //当访问一个对象的属性和方法时，先在自身中寻找
        //如果没有，就会沿着__proto__这条链，像上查找，一直找到最顶层Object.prototype为止

        Object.prototype.__proto__ === null


        //数组的原型链
        const arr = [1,2,3]  //new Array()
        //数组也是一个对象，它有Array这个构造函数创建出来的
        console.log(arr.__proto__ === Array.prototype)
        //arr ---> Arrat.prototype ---> Object.prototype ---> null

        //函数的原型链
        const fn = function(){} //new Function()
        //函数也是一个对象，它由Function构造函数创建
        //fn --->Function.prototype ---> Object.prototype --->null
```

### 5.instanceof

```js
  // instanceof  运算符  instance 实例 of谁的

        // 语法： A instanceof B
        //       实例 instanceof 构造函数
        // 作用： 用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上

        function C(){}
        const o = new C()
        console.log(o.__proto__ === C.prototype)

        console.log(o instanceof C)
        console.log(o instanceof Object)

        const arr = [1, 2, 3]
        console.log(arr instanceof Array)
        console.log(arr instanceof Object)
        // console.log(arr instanceof null) // Error 
        console.log(Array instanceof Object)

        console.log(C instanceof Function)
```

---

# js高级day4

## 一、原型

### 1.1 什么是原型

原型就是一个对象

  1.每个函数都有prototype属性 它的值是一个指针 指向的就是原型对象

  2.通过构造函数生成的实例都有`__proto__` 属性 也指向原型对象

  3.原型上默认有有个属性constructor属性 指回构造函数

```js
function A(){}
    const a = new A()
    a.__proto__ === A.prototype

    // 原型的结构
    A.prototype = {
      constructor : A,
    // ...其他其他原型属性和方法
    }
    a.__proto__ = {
      constructor : A,
    // ...其他其他原型属性和方法
    }
```

原型的作用

所有通过构造函数创建的实例 都共享原型上的属性和方法

### 1.2原型链继承

```js
//类：抽象的，一个大类，泛指 ===> 构造函数可以理解为一个类
        //继承：子类拥有（继承了）父类的属性和方法

        //实现继承的方式
        //============================
        // 1.原型链继承  ===>今天学的
        // 2. 借用构造函数（经典继承）
        // 3. 组合继承
        // 4. 原型式继承
        // 5. 寄生式继承
        // 6. 寄生式组合继承 ==> 最完美的没有缺点
        // =====================================================

    	// ES6.
        // 7. extends 继承 ==> 语法糖 寄生式组合继承
```

```js
// 继承: 子类拥有父类的属性和方法
    // 父类
    function Parent(name){
      this.name = name || '父类'
      this.arr = [1, 2, 3]
    }
    Parent.prototype.sayHi = function(){
      console.log('Hi`');
    }
    // 子类
    function Child(hobby){
      this.hobby = hobby
    }
    // 核心: 让子类的原型 等于 父类构造函数的实例
    Child.prototype = new Parent()

    const boy1 = new Child('sing')
    const boy2 = new Child('dance')
    console.log(boy1)
    console.log(boy2)
    boy1.sayHi()
    boy2.sayHi()
    console.log(boy1.sayHi === boy2.sayHi)

    // 1. 优点 方法复用,共享
    // 由于方法定义在父类的原型上,实例共享了父类原型上的方法


    // 2. 缺点
    // 2.1 子类在实例化的时候 不能给父类构造函数传参
    console.log(boy1.name)
    console.log(boy2.name)
    console.log(boy1.name === boy2.name)
    // 2.2 子类实例共享了父类构造函数的属性和方法
    // 如果父类里的属性值是引用类型(数组,对象等) 实例修改这个值后会相互影响
    console.log(boy1.arr,boy2.arr)
    boy1.arr.push(666)
    console.log(boy2.arr)


    // 注意1: 这里如果写boy1.那么 ,相当于给自己的实例添加一个name属性
    boy1.name = 666
    console.log(boy1.__proto__.name)
    
    console.log(boy1,boy2)

    // 注意2: 我们希望这里应该返回的是child,可以修正
    console.log(boy1.constructor) // Parent
    Child.prototype.constructor = Child // 修正child里面的constructor属性
```



## 二、数据类型

### 2.1 基本数据类型 (面试题)

Number String  Boolean undefined null Symbol BigInt

栈（Stack）基本类型：基本类型的值，存在栈里面

```js
 // 1. 基本数据类型不可以添加属性和方法
    let str = '123'
    str.a = '222'
    str.max = function(){}
    console.log(str);
    console.log(str.a); // undefined
    console.log(str.max); // undefined
    // 2. 基本数据类型是按值访问的 也就是说 我们操作的是存储在变量中的实际值
    let a = 1
    let b = 2

    a = 3
    b = 4
    a = b // 互不干扰
    console.log(a,b)
    // 基本类型: 赋值后的两个变量,互不干扰
```



### 2.2  引用类型 （面试题）

Object ==> Function Array Date Math Error RegExp

堆（Heap）引用类型： 引用类型的数据，栈里面存的是地址，指向堆里面的数据。

```js
// 3. 引用类型: 直接赋值,实际上是复制的地址 (指针)
    let obj1 = {name: '刘德华',age:'18'}
    let obj2 = obj1
    obj1.name = '不能再迟到'
    console.log(obj2)
```

## 三、检测数据类型

### 3.1 typeof

```js
 // 1. typeof 判断一个变量是否是除了null以外的基本数据类型（原始类型）
        // 基本类型
        console.log(typeof 1);
        console.log(typeof "");
        console.log(typeof true);
        console.log(typeof undefined);
        console.log(typeof null); // object---有点儿特殊 Bug
        console.log(typeof Symbol('id')) // symbol
        console.log(typeof BigInt(9007199254740999)) // bigint
        console.log(typeof 9007199254740999n)

        // 2. typeof 总是返回一个字符串
        console.log(typeof typeof 1) // string

        // 3. typeof 不能区分数组
        console.log(typeof [1, 2, 3]) // 'object'
        console.log(typeof function(){}) // 'function'
        console.log(typeof {}) // 'object'
```



### 3.2 instance of

```js
// 1. instanceof 可以用于引用类型的检测，对基本数据类型无效
        console.log('1' instanceof String) 
        console.log(1 instanceof Number)
        console.log(true instanceof Boolean)
        console.log(Symbol('id') instanceof Symbol)
        console.log(9007199254740999n instanceof BigInt)
        console.log(BigInt(9007199254740999) instanceof BigInt)

        // 引用类型
        // 用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上
        // ==> 用于检测左边的实例是不是构造函数创建的，或者说是不是属于祖先构造函数。
        console.log([] instanceof Array)  // true
        console.log(function () {} instanceof Function) // true
        console.log({} instanceof Object)  // true
        console.log([] instanceof Object) // true 祖先
```

### 3.3 Object.prototype.toString.call()

```js
 let num = 123
        let str = '123'
        let bool = true
        let und = undefined
        let nul = null
        let symb = Symbol('id')
        let big = 9007199254740999n

        let arr = [1,2,3]
        let obj = {}
        let fn = function(){}
        let date = new Date()   // [object Date]
        let reg = /a/g          // [object RegExp]
        let error = new Error() // [object Error]
        // 前面的object小写, 后面的类型, 大写开头; 字符串
        console.log(Object.prototype.toString.call(num)) // [object Number]
        console.log(Object.prototype.toString.call(str))  // [object String]
        console.log(Object.prototype.toString.call(bool)) // [object Boolean]
        console.log(Object.prototype.toString.call(und)) // [object Undefined]
        console.log(Object.prototype.toString.call(nul)) // [object Null]
        console.log(Object.prototype.toString.call(symb)) // [object Symbol]
        console.log(Object.prototype.toString.call(big)) // [object BigInt]

        console.log(Object.prototype.toString.call(arr))  // [object Array]
        console.log(Object.prototype.toString.call(obj))  // [object Object]
        console.log(Object.prototype.toString.call(fn))   // [object Function]
        console.log(Object.prototype.toString.call(date))   // [object Date]
        console.log(Object.prototype.toString.call(reg))   // [object RegExp]
        console.log(Object.prototype.toString.call(error))   // [object Error]

        //封装一个函数checkType函数，检测数据类型
        const tempArr = [num, str, bool, und, nul, symb, big, arr, obj, fn, date, reg, error]
        function foo(tempArr){
            tempArr.forEach(el => {
              Object.prototype.toString.call(el)  
            })
        }
        console.log(foo(tempArr))

        //=============================
        // 判断数组
        // instanceof 
        // Array.isArray
        // Object.prototype.toString.call()
```



## 四、赋值

什么是赋值？

- 赋值是将某一数值或对象赋给某个变量的过程

```js
 let a = '莫听穿林打叶声，何妨吟啸且徐行' // 自行百度
    let b = a 
    console.log(b)

    a = '贵有恒，何必三更起五更眠'
    console.log(a, b)

    // 引用类型
    let obj1 = {
        name: "JS",
        book: {
            title: "You Don't Know JS",
            price: "169"
        }
    }
    let obj2 = obj1
    obj2.name = 'Hello World'
    // console.log(obj1.name)
    console.log(obj1, obj2)
```

## 五、拷贝

### 5.1 浅拷贝

浅拷贝：在堆内存中新开辟一个内存空间，创建一个新对象

拷贝原对象的第一层基本数据类型的值和引用类型的地址

第一层：最外面的一层，{}最近的一层

```js
    // 实现浅拷贝的方式
    // 1. Object.assign() 
    const b = Object.assign({}, obj1)
    console.log(b)
    b.name = 'hello world'
    b.book.price = 666
    console.log(obj1, b)

    console.log(b.name === obj1.name) // false
    console.log(b.book === obj1.book) // true
    console.log(b.book.price === obj1.book.price) // true

    // 2. 扩展运算符 Spread ...
    let c = {
        name: "JS",
        book: {
            title: "You Don't Know JS",
            price: "169"
        }
    }
    let d = {...c} 
    // console.log(d)
    d.name = 'hello world'
    const e = 'JS'
    const f = 'JS'
    console.log(e === f) // 基本数据类型比较全等，是比较的值和类型
    
    console.log(c.name === d.name)
    console.log(c.book === d.book)
    c.book.price = 888
    console.log(d.book.price)

    // 3. Array.prototype.concat()
    const arr1 = [1, 2, {user:'周杰伦'}]
    const arr2 = arr1.concat()
    arr2[0] = 666
    arr2[2].user = '练练'
    console.log(arr1, arr2)

    // 4. Array.prototype.slice()
    const arr3 = [1, 2, {user:'周杰伦'}]
    const arr4 = arr3.slice()
    arr4[0] = 888
    arr4[2].user = '伟伟'
    console.log(arr3, arr4)
```



### 5.2 深拷贝

深拷贝:  在堆内存空中 开辟一个新的空间 存放新对象

递归的拷贝原对象的所有属性和方法

 拷贝前后 两个对象,相互不影响

 递归拷贝: 一层一层,每一层都新建一个内存空间

   ==深拷贝的实现方式 面试高频==

1. ==JSON.parse(JSON.stringify(obj))==

2. ==递归实现==

3. ==使用一些js库 lodash==

```js
//浅拷贝：新旧对象，如果有引用类型，共享同一块内存空间，会相互影响
  let a = {b:{c:{}}}
  let a2 = {...a} //

  //深拷贝：新旧对象，不会共享内存空间
  let a3 = deepClone(a)
  a3.b.c === a.b.c //false 不共享内存空间
```



## 六、递归

 递归：自己调用自己 ，有递，有归。

递归爆栈(栈溢出) 栈 ==> 数据结构 有先入后出的特性,栈有深度 如果超出了 就会溢出报错

```js
let i = 1
        function count(){
            console.log(`这是第${i}次调用`)
            i++
            // if(i >= 100){
            //     return
            // }
            count()
        }
        count()
```



## 七、指向

### 7.1 基本概念

 this 指向  ===>  指向 可以理解为 等于 代表谁

 this在定义的时候不能确定, 只有执行调用的时候才能确定

 全局作用域中 / 普通函数中 / 定时器里面  this 指向 window

```js
 // 1.1 全局作用域
        console.log(this)  // window

        // 1.2 普通函数调用
        function fn() {
        console.log('大家吃早饭了嘛?')
        console.log(this) // window
        }
        fn()

        // 1.3 定时器里面
        setTimeout(function(){
        console.log(this)  // window
        }, 1000)

        // 2.1 方法调用中, 谁调用这个方法, this指向谁
        const obj = {
        name: '小平',
        age: 18,
        sayHi: function(){
            console.log(this)
        }
        }
        obj.sayHi()

        // 2.2 事件注册的时候, this指向被绑定的元素
        const btn = document.querySelector('button')
        btn.addEventListener('click', function(e){
        console.log(this) // 绑定事件的元素
        console.log(e.target) // 触发事件的元素
        console.log(e.currentTarget) // 同this, 绑定事件的元素
        })

        console.log('-------------------------')

        // 3. 构造函数中, this 指向的是 构造函数的实例
        function Foo(name, age) {
        this.name = name 
        this.age = age 
        console.log(this)
        }
        const person1 = new Foo('小红', 19)
        const person2 = new Foo('小白', 18)

        // 4. 原型方法里面, this指向实例.
```

```js
 // 1. 箭头函数没有prototype 原型
       let a = () => {}
        console.log(a.prototype) // undefined

        // 2. 不能使用new调用箭头函数，也就是箭头函数不能作为构造函数
        let b = () => {}
        // let c = new b() // b is not a constructor
        // console.log(c)

        // 3. 箭头函数没有arguments， 可以利用rest接收剩余参数
        let d = (first, ...abc) => {
            console.log(first, abc) // 1, [2, 3, 4]
        }
        d(1, 2, 3, 4)

        // 4. 箭头函数本身没有this
        // 箭头函数的this在定义的时候就确定了，上层作用域中的this
        // 箭头函数的this指向是固定的，不能改变。
    
        let msg = '666' //let声明的变量不挂载到window上
        let obj = {
            msg:'xxx',
            say:function(){
                console.log('say：', this.msg)
            },
            sayHi: () => {
                // window
                console.log('sayHi：', this.msg)
            }
        }
        obj.say()  // say: xxx
        obj.sayHi() // sayHi: undefined '
```

### 7.2 改变指向的三种方式

#### 7.2.1 fn.call()

1. 改变this的指向

2. 调用函数,函数立即执行 返回执行结果

3. 注意:第一个参数是要让this指向那个对象

   4.后面的紧跟的是参数列表

```js
 const obj = {
      msg : 'hallo world'
    }
    const b = {
      demo : 'hahaha'
    }
    function fn(x,y){
      console.log(this)
      console.log(this.msg)
      console.log(x + y)
    }
    // fn(1,2)
    fn.call(obj , 1,2) // 改变了this的指向让this指向了obj
```

```js
const obj2 = {
            name:'hw',
            getName(){
                console.log(this)  // obj3
                console.log(this.name) // iPhone
             }
        }
        const obj3 = {
            name:'iPhone'
        }
        obj2.getName.call(obj3) 
```

#### 7.2.2  fn.apply() 

​    1. 改变this的指向

​    2. 调用函数，立即执行

​    3. 返回值就是函数本身的返回值

```js
 const obj = {
            msg:'hello world'
        }

        function fn(x, y){
            console.log(this.msg)
            return x + y
        }

        // fn.apply(this指向谁,[数组])
        const res = fn.apply(obj, [1, 2])
        console.log(res)
```

#### 7.2.3 fn.bind(obj, 参数列表)

1. 改变this指向

2. 有返回值，返回值就是一个函数，这个函数里面的this就是我们传入的第一个参数

3. bind不是立即执行的， 需要手动调用

```js
 const fun = fn.bind(obj, 1, 2)
        // const fun = function fn(x, y){
        //     console.log(this.msg)
        //     return x + y
        // }
        // 我们用fun去接收了这个函数， 然后手动调用的fun()
        console.log(fun)
        const res = fun()
        console.log(res)
```

> 总结

​    // call / apply / bind 区别

​    // 1. 都是改变this的指向

​    // 2. call接收的是参数列表 ， apply接收的是数组

​    // 3. call和apply是立即执行， bind返回一个函数，需要手动调用

---

## 八、手写递归深拷贝

```js
const obj = {
            name:'雪豹',
            age:18,
            like:['smoke','fuck'],
            say:{
                people:'我测你们码'
            }
        }

        const o = {}

        function deepClone(newObj,oldObj){
            for(let k in oldObj){
                if(oldObj[k] instanceof Array){
                    newObj[k] = []
                    deepClone(newObj[k],oldObj[k])
                }
                else{
                    newObj[k] = oldObj[k]
                }
            }
        }
        deepClone(o,obj)
        console.log(o)
```

