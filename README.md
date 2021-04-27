# js_learn
js前端基础进阶

# 前端基础进阶（一）：内存空间
## 堆(heap)，栈(stack)与队列(queue)：
1. 栈数据结构（先进后出，后进先出）
2. 堆数据结构（树状结构，可以无序）
3. 队列（先进先出<FIFO>的数据结构）
4. 基础数据类型与变量对象（Undefined、Null、Boolean、Number、String、Symbol）
5. 引用数据类型与堆内存（object包含：function、Array、Date）
![image](https://user-images.githubusercontent.com/42436934/116209784-d49c7380-a774-11eb-8873-f6f02373d517.png)
## JavaScript的内存生命周期
1. 分配你所需要的内存
2. 使用分配到的内存（读、写）
3. 不需要时将其释放、归还
## 垃圾收集机制
- 其实很简单，就是找出那些不再继续使用的值，然后释放其占用的内存。垃圾收集器会每隔固定的时间段就执行一次释放操作。
- 在JavaScript中，最常用的是通过标记清除的算法来找到哪些对象是不再继续使用的，因此a = null其实仅仅只是做了一个释放引用的操作，让 a 原本对应的值失去引用，脱离执行环境，这个值会在下一次垃圾收集器执行操作时被找到并释放。而在适当的时候解除引用，是为页面获得更好性能的一个重要方式。
- 在局部作用域中，当函数执行完毕，局部变量也就没有存在的必要了，因此垃圾收集器很容易做出判断并回收。但是全局变量什么时候需要自动释放内存空间则很难判断，因此在我们的开发中，需要尽量避免使用全局变量。

# 前端基础进阶（二）：执行上下文
![image](https://user-images.githubusercontent.com/42436934/116209914-f4cc3280-a774-11eb-81a8-6fb1e7a19827.png)
## JavaScript中的运行环境全局环境：
- JavaScript代码运行起来会首先进入该环境
- 函数环境：当函数被调用执行时，会进入当前函数中执行代码
- eval（不建议使用，可忽略）
## 执行上下文特点：
- 单线程
- 同步执行，只有栈顶的上下文处于执行中，其他上下文需要等待
- 全局上下文只有唯一的一个，它在浏览器关闭时出栈
- 函数的执行上下文的个数没有限制
- 每次某个函数被调用，就会有个新的执行上下文为其创建，即使是调用的自身函数，也是如此

# 前端基础进阶（三）：变量对象详解
![image](https://user-images.githubusercontent.com/42436934/116210019-10373d80-a775-11eb-8c4c-41a48557c4ce.png)
## 执行上下文生命周期
当调用一个函数时（激活），一个新的执行上下文就会被创建。而一个执行上下文的生命周期可以分为两个阶段。
- 创建阶段（在这个阶段中，执行上下文会分别创建变量对象，建立作用域链，以及确定this的指向。）
- 代码执行阶段（创建完成之后，就会开始执行代码，这个时候，会完成变量赋值，函数引用，以及执行其他代码。）
![image](https://user-images.githubusercontent.com/42436934/116210071-1fb68680-a775-11eb-83fe-7b7d95aa5664.png)
## 变量对象的创建，依次经历了以下几个过程：
1. 建立arguments对象。检查当前上下文中的参数，建立该对象下的属性与属性值。
2. 检查当前上下文的函数声明，也就是使用function关键字声明的函数。在变量对象中以函数名建立一个属性，属性值为指向该函数所在内存地址的引用。如果函数名的属性已经存在，那么该属性将会被新的引用所覆盖。
3. 检查当前上下文中的变量声明，每找到一个变量声明，就在变量对象中以变量名建立一个属性，属性值为undefined。如果该变量名的属性已经存在，为了防止同名的函数被修改为undefined，则会直接跳过，原属性值不会被修改。
![image](https://user-images.githubusercontent.com/42436934/116210440-7328d480-a775-11eb-885e-713fab743812.png)
#### 注：上面的三条规则仅仅适用于变量对象的创建过程，也就是执行上下文的创建过程。

# 前端基础进阶（四）：作用域链与闭包
## 了解作用域与作用域链，需要先明白以下重要概念：
- 基础数据类型与引用数据类型
- 内存空间
- 垃圾回收机制
- 执行上下文
- 变量对象与活动对象
## 作用域
- 在JavaScript中，我们可以将作用域定义为一套规则,这套规则用来管理引擎如何在当前作用域以及嵌套的子作用域中根据标识符（变量名或者函数名）进行变量查找。
- 在JavaScript中，只有全局作用域与函数作用域（因为eval我们平时开发中几乎不会用到它，这里不讨论）。
- 作用域与执行上下文是完全不同的两个概念。我知道很多人会混淆他们，但是一定要仔细区分。
- JavaScript代码的整个执行过程，分为两个阶段，代码编译阶段与代码执行阶段。
![image](https://user-images.githubusercontent.com/42436934/116204205-1cb89780-a76f-11eb-89e1-6b363ae4bc6d.png)
## 作用域链
作用域链，是由当前环境与上层环境的一系列变量对象组成，它保证了当前执行环境对符合访问权限的变量和函数的有序访问。

