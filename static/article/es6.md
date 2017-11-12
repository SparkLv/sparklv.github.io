# ES6

## Babel转换器

配置文件为.babelrc，使用babel工具和模块需要先写好这个配置文件

### babel-cli工具

#### 转码结果写入一个文件

```javascript
$ babel a.js --out-file b.js
或
$ babel a.js -o b.js
```

##### 整个目录转码

```javascript
$ babel src --out-dir lib
或
$ babel src -d lib
```

##### 需要将babel-cli安装在项目之中

```javascript
$ npm install --save-dev babel-cli
package-json
'devDependencies':{
    "babel-cli":'xxxx'
}
"script":{
    "build":"babel src -d lib"
}
```

最后
`$ npm run build`

## let和const命令

### let

let声明的变量只在let命令所在的代码块内有效
在for循环中，当前let的索引仅在本轮循环有效，所以每一次循环的索引都是一个新的变量

#### let不存在变量提升

var命令的变量提升，变量可以在声明之前使用，值为undefined
let声明的变量一定要在声明之后使用，否则会报错。ES6明确规定，如果区块中存在let和const命令，在声明之前就使用这些变量，就会报错

#### let不允许在相同作用域内，重复声明同一变量

ES6允许块级作用域的任意嵌套，内层作用域可以定义外层作用域的同名变量

### const

声明一个只读的常量，一旦声明，常量的值就不能改变,而且必须立即初始化，和let相同，只在声明所在的块级作用域内有效
如果const声明的是一个数组或者对象，那么可以进行添加属性等操作。但不可以赋值

## 变量的解构赋值

ES6允许 `let [a,b,c] = [1,2,3];`
如果左右不对应，解构会失败，但是会部分成功，称作不完全解构

### 解构赋值允许指定默认值

```javascript
let[foo = true] = []
let [x = 1] = [undefined]
let [x = 1] = [null]
```

ES6采用严格的相等运算符"===",如果一个数组成员不严格等于undefined，默认值不会生效
如果默认值是一个表达式，那么这个表达式是惰性求值的，即只有在用到的时候才会求值

### 对象解构赋值

对象的解构赋值和数组不同，数组是按次序排列，由位置决定。而对象必须与属性同名才能取值，如果变量名和对象名不同使用

```javascript
let{b:l}={b:111}
```

### 字符串解构赋值

```javascript
const [a,b,c,d,e] = 'hello';
```

## 字符串扩展

ES6为字符串提供了遍历接口，使得字符串可以被for...of循环遍历

### includes(),startsWith(),endsWith()

作用分别是，是否找到了参数字符串，参数字符串是否在原字符串的头部，参数字符串是否在原字符串的尾部。
这些方法都支持第二个参数，表示搜索位置，但是endsWith表示的是针对前n个字符

### repeat

返回一个新字符串，表示将原字符串重复n次

## 函数的扩展

ES6允许为函数的参数设置默认值，即直接写在参数定义的后面。要注意的是，参数变量是默认声明的，所以不能用let和const再次声明。另外，函数不能有同名参数

### length属性

将返回函数没有指定默认值的参数个数
如果设置了默认值的参数不是尾参数，那么length属性也不再计入后面的参数

### 作用域

一旦设置了参数的默认值，函数进行声明初始化时，参数会形成一个单独的作用域。等到初始化结束，这个作用域就会消失。

### rest参数

rest参数的形式为 ...变量名（是个数组） 用于获取函数的多余参数，这样就不需要使用arguments对象，对比一下两种方法

```javascript
function sortNumber(){
    return Array.prototype.slice.call(arguments).sort();
}
const sortNumbers = (...numbers)=>numbers.sort();
```

#### 注意，rest参数之后不能再有其他参数，否则会报错

### name属性

函数的name属性，用于返回该函数的函数名

与es5不同的是，es6中函数表达式的name是变量名

Function构造函数返回的函数实例，name属性的值为anonymous

bind返回的函数，name属性值会加上bound前缀

### new.target

如果是使用new调用的函数，在函数体内部使用new.target，返回的是正在执行的函数，如果不是new调用，则指向undefined

### 箭头函数

ES6 允许使用箭头"=>"定义函数

例如:`var f = v => v;`

* 如果箭头函数不需要参数或者需要多个参数，就是用圆括号来代表参数部分

```javascript
var f = () => 5;
var sum = (num1,num2) => num1
```

* 如果箭头函数的代码部分多于一条语句，就要使用大括号将它们括起来，并使用return语句返回

```javascript
var sum = (num1,num2) => {num1++;num2++;return num1 + num2}
```

* 由于箭头函数直接返回一个对象，返回对象必须在外面加上括号

* 箭头函数的函数体内的this对象，指向的就是定义时所在的对象，而不是使用时

* 所在的对象（所以，箭头函数没有匿名函数指向window对象的说法）

* 箭头函数不可以当作构造函数，也就是不可以使用new命令

* 箭头函数不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以使用rest参数代替

## 数组的扩展

### 扩展运算符

是三个点(...)，好比rest参数的逆运算，将一个数组转为用逗号分隔的参数序列

#### 复制数组

```javascript
const a1 = [1,2];
const a2 = [...a1];
或 conse [...a2] = a1
```

#### 合并数组

`[1,2,...more]`

### Array.from()

用于将两类对象转化为真正的数组，一种是类似数组的对象，另一种是可遍历的对象。它还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组。

### Array.of()

用于将一组值转换为数组（因为当Array构造函数没有参数和有一个参数或多个参时，行为会有差异），当Array.of()没有参数，就返回一个空数组。

### 数组实例的find和findIndex

参数为一个回调函数（参数依次为，当前值、当前位置、原数组），对数组所有成员使用这个函数，直到出现第一个符合条件的数组成员或者位置

这两个方法都可以发现NaN弥补了IndexOf方法的不足

## Promise