## JavaScript核心简化版 ##

[###原文点击这里###][1]

####对象####
![对象示意图][2]

    一个对象就是一个属性集合，并拥有一个独立的prototype（原型）对象。这个prototype可以是一个对象或者null。


----------


####原型链####
![原型链示意图][3]

    原型链是一个用来实现继承和共享属性的有限对象链。


----------


####构造函数####
![构造函数示意图][4]

    自动地为新创建的对象设置一个原型对象。这个原型对象存储在ConstructorFunction.prototype属性中


----------


####执行上下文栈####
![ec-stack][5]
![ec-stack-changes][6]


----------


####执行上下文####
![执行上下文示意图][7]

    一个执行上下文可以抽象的表示为一个简单的对象。每一个执行上下文拥有一些属性用来跟踪和它相关的代码的执行过程


----------


####变量对象####
![变量对象示意图][8]

    变量对象是与执行上下文相关的数据作用域。它是一个与上下文相关的特殊对象，其中存储了在上下文中定义的变量和函数声明。


----------


####活动对象####
![活动对象示意图][9]


----------


####作用域链####
![scope-chain][10]
![scope-chain-with][11]

    作用域链是一个对象列表，上下文代码中出现的标识符在这个列表中进行查找。


----------


####闭包####
![闭包示意图][12]

    闭包是一个代码块（在ECMAScript是一个函数）和以静态方式/词法方式进行存储的所有父作用域的一个集合体。所以，通过这些存储的作用域，函数可以很容易的找到自由变量。
    


----------


####This####

    this是一个与执行上下文相关的特殊对象。因此，它可以叫作上下文对象（也就是用来指明执行上下文是在哪个上下文中被触发的对象）。
this是执行上下文的一个属性，而不是变量对象的一个属性


----------


  [1]: http://dmitrysoshnikov.com/ecmascript/javascript-the-core/ "原文"
  [2]: https://raw.githubusercontent.com/yanboweb/blog-images-store/master/20170626/basic-object.png
  [3]: https://raw.githubusercontent.com/yanboweb/blog-images-store/master/20170626/prototype-chain.png
  [4]: https://raw.githubusercontent.com/yanboweb/blog-images-store/master/20170626/constructor-proto-chain.png
  [5]: https://raw.githubusercontent.com/yanboweb/blog-images-store/master/20170626/ec-stack.png
  [6]: https://raw.githubusercontent.com/yanboweb/blog-images-store/master/20170626/ec-stack-changes.png
  [7]: https://raw.githubusercontent.com/yanboweb/blog-images-store/master/20170626/execution-context.png
  [8]: https://raw.githubusercontent.com/yanboweb/blog-images-store/master/20170626/variable-object.png
  [9]: https://raw.githubusercontent.com/yanboweb/blog-images-store/master/20170626/activation-object.png
  [10]: https://raw.githubusercontent.com/yanboweb/blog-images-store/master/20170626/scope-chain.png
  [11]: https://raw.githubusercontent.com/yanboweb/blog-images-store/master/20170626/scope-chain-with.png
  [12]: https://raw.githubusercontent.com/yanboweb/blog-images-store/master/20170626/shared-scope.png