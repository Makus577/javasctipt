ES5只有全局作用域和函数作用域，没有块级作用域。

var第一种出现的场景就是内层变量会覆盖外层变量

另外一个var带来的不合理场景就是用来计数的循环变量泄漏为全局变量（闭包解决）

let 类似var，为变量

const 类似var，为常量，一旦声明，常量的值就不能改变，当改变const声明的常量时，浏览器就会报错。引用第三方库时声明的变量，用const声明可以避免未来不小心重命名而导致出现bug。



原型、构造函数、继承、指针（this）的指向。。。

constructor（构造方法），this关键字则代表实例对象。

constructor定义的方法和属性时实例对象自己的，而之外定义的方法和属性时所有实例对象可以共享的。

class

extends 继承父类的所有属性和方法

super 关键字指代父类的实例（即父类的this对象），子类必须在constructor方法中调用super方法，否则新建实例时会报错，这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其加工，如果不掉用super方法，子类就得不到this对象。

ES6的继承机制，实质是先创造父类的实例对象this（所以必须先调用super方法），然后再用子类的构造函数修改this。



arrow function

this的指向。。

一种是this传给self，再用self指代this

一种是bind（this）

箭头函数，函数体内的this对象，就是定义时所在对象，而不是使用时所在对象。

并不是因为箭头函数内部有绑定this的机制，实际原因是箭头函数根本没有自己的this，他的this是继承外面的，因此内部的this就是外层代码块的this。



引发思考：

class Animal {
​    constructor(){
​        this.type = 'animal'
​    }
​    says(say){
​        setTimeout(function(){
​            console.log(`this`.type + ' says ' + say)
​        }, 1000)
​    }
}

 var animal = new Animal()
 animal.says('hi')  //undefined says hi

1. 上面this指向的是全局对象，但是是因为setTimeout的指向嘛？
2. 箭头函数的this指向，为定义时所在对象，何谓定义？对上例子分析？

Template string

<Link to = {`/tao/${tao.name}`}>{tao.name}</Link>

 destrucuring

ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构

let cat = 'ken'
let dog = 'lili'
let zoo = {cat: cat, dog: dog} // ES5

Let zoo = {cat, dog}

console.log(zoo)  //Object {cat: "ken", dog: "lili"}



let dog = {type: 'animal', many: 2}
let { type, many} = dog

defalut

rest      …types

 arguments





Import export

一方面js代码变得很臃肿，难以维护

另一方面我们常常注意每个script标签再html中的位置，因为她们通常有依赖关系，顺序错了可能会出bug

commonJs（服务器端）和AMD（浏览器端，如require.js）

ES6模块的设计思想，是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonJS和AMD模块，都只能在运行时确定这些关系。

require.js写法,index.js和content.js

现在在index.js中使用content.js返回的结果

//content.js

define('content.js', function() {

​	return 'A cat'

})

//index.js

require(['./content.js'], function(animal) {

​	console.log(animal); // A cat

 })

CommonJS写法

//index.js

var animal = require('./content.js');

//content.js

Module.exports = 'A cat';



ES6写法

//index.js

Import animal from './content';

//content.js

export default 'A cat';



ES6 module的其他高级用法

//content.js

Export default 'A cat';

Export function say() {

​	return 'Hello !';

}

Export const type = 'dog'

export命令除了输出变量，还可以输出函数，甚至是类（react的模块基本都是输出类）

//index.js

import { say, type} from './content';／／大括号里面的变量名，必须与被导入模块对外接口的名称相同

let says = say();

Console.log(`The ${type} say $(says)`);

如果希望输入content.js中输出的默认值（defalut）,可以写在大括号外面

import animal , { say , type }  from './content'



修改变量名

此时我们不喜欢type这个变量名，因为它有可能重名，在es6中可以用as实现一键换名

//index.js

import animal, { say, type as animalType } from './content'



模块的整体加载

除了指定加载某个输出值，还可以使用整体加载，即用星号（*）指定一个对象，所有输出值都加载在这个对象上面

//index.js

import animal, * as content from './content'

通常星号*结合as一起使用比较合适



##### 终极秘籍

考虑下面的场景：上面的`content.js`一共输出了三个变量（default, say, type）,假如我们的实际项目当中只需要用到type这一个变量，其余两个我们暂时不需要。我们可以只输入一个变量：

```
import { type } from './content'

```

由于其他两个变量没有被使用，我们希望代码打包的时候也忽略它们，抛弃它们，这样在大项目中可以显著减少文件的体积。

ES6帮我们实现了！

不过，目前无论是webpack还是browserify都还不支持这一功能...

如果你现在就想实现这一功能的话，可以尝试使用rollup.js

他们把这个功能叫做Tree-shaking，哈哈哈，意思就是打包前让整个文档树抖一抖，把那些并未被依赖或使用的东西统统抖落下去。。。

看看他们官方的解释吧：

> Normally if you require a module, you import the whole thing. ES2015 lets you just import the bits you need, without mucking around with custom builds. It's a revolution in how we use libraries in JavaScript, and it's happening right now.





何为闭包？

当子函数引用了父函数的属性的时候就形成了闭包