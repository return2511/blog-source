---
title: 设计模式系列之基础知识
date: 2020-6-16 01:01:00
tags: [js, design pattern]
categories: js
---

> 设计模式基础知识和设计模式原则

<!-- more -->

### 什么是设计模式

> 定义： 在面向对象软件设计过程中针对特定问题的简洁而优雅的解决方案。

设计模式（Design pattern）是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性。设计模式使代码编制真正工程化，设计模式是软件工程的基石，如同大厦的一块块砖石一样。

#### 听不懂？ 说人话

说白了就是我们编码过程实现一个需求，编码方式有千千万万种。但何为“最佳实践”呢？
> 别跟我讲什么设计模式，老夫写代码就是一把梭， 抄起键盘就是ctrl+C/ctrl+V

别笑，我相信很多人写代码都经历过这个阶段，但为何要笑呢？笑的原因是觉得这样不好，但为啥不好呢？解决上面哪个问题的“好的方案”其实通俗点就是设计模式。

#### 个人理解

- 按照一种思路或者说是一种标准来实现所需功能，这种标准是行业内很多大佬总结出来的所谓“最佳实践”。就像高中做数学题，可能会有很多种解法，但是班级里的优等生经常会有各种巧解的一些简单方法。
- 分析需求，不同的场景可能需要对应不同的解决方案（设计模式一般的资料上会讲23种）。打架之前先挑一把趁手的家伙，别随便摸到一个就上。
- 小的项目（业务不复杂）可以不使用设计模式，举个栗子，如果搭建一面矮墙，普通的建筑工人半天时间足矣。但如果是盖一座高楼大厦，再多的普通建筑工人，如果没有图纸和工程师的规划等，可能都很难实现。就算实现了，安全性等各方面也无法保证。

### 设计模式的原则

做啥事都有原则，同样的设计模式也有其基础的原则，在实现设计模式的时候最好在保证这些基础原则的基础上。

- 单一职责原则（Single responsibility principle）
- 开放封闭原则（Open Closed Principle）
- 里氏置换原则（Liskov Substitution Principle, LSP）
- 接口独立原则（Interface Segregation Principle）
- 依赖倒置原则（Dependence Inversion Principle）
- 迪米特法则（Law of Demeter,简称LoD）

#### 单一职责原则 SRP

一个类（方法）只负责一个职责，只有一个引起它变化的原因，提高内聚，降低耦合。

举个栗子，已近中年的我是不太能很好的理解现在明星这个行业的要求了，毕竟我心中的idol是要唱歌好听，长得好看，演技在线，等指标，可是有一天你告诉我现在明星都要会唱、跳、Rap、篮球？

这里面我们来写一个明星制造器

``` javascript
class Idol {
    constructor(type) {
        this.type = type
    }

    sing() {
        if (this.type === 'old') console.log('I am a singer')
        if (this.type === 'new') console.log('I think my music is NO. 1')
    }

    acting() {
        if (this.type === 'old') console.log('my acting is very good!')
        if (this.type === 'new') console.log('what is acting, you known nothing about acting!')
    }

    faceScore() {
        if (this.type === 'old') console.log('my face score is full mark!')
        if (this.type === 'new') console.log('I am the most beautiful idol!s')
    }
}

const idol = new Idol('old')
```

无意抨击任何时代的idol，栗子就是说当我们实现一个idol的某个能力的时候，应该单独做一个方法去实现，好处也在代码中体现了，我们可以根据type来判断当前idol对某一方面才华的不同实现。 也保证了 sing / acting 等各种技能的耦合，一个方法只做一件事情。

再举一个js种用到更多的栗子。单一职责和compose的结合使用，假设有一个需求： 对一个字符串（纯字母），实现将这个字符串转换为大写，然后逆序

``` javascript
// 常规写法
let str = 'asdfghjkl'
function doSth(str) {
    const upperStr = str.toUpperCase()
    return upperStr.split('').reverse().join('')
}
console.log(doSth(str))
```

看起来上面的实现没有任何问题，但是，假设我们的需求变成了要返回数组怎么办？改变原来的function？在大型的开发项目中，改变一个已经实现的function往往是一个大忌。为啥？你知道有多少人用这个方法么？

``` javascript
let str = 'asdfghjkl'
const strToUpper = str => str.toUpperCase()

const strReverse = str => str.split('').reverse().join('')

let toUpperAndReverse = compose(strToUpper, strReverse)

// 使用reduce实现 compose function
function compose(...args) {
    return (x) => args.reduce((arg, fn) => fn(arg), x)
}
```

上面的实现好处就是能够根据实际需求定制功能，而对之前实现的方法不修改（这也是后面要提到的另外一个原则）。

如果需要返回结果是一个array，那么只需要在compose里面再增加一个字符串转array的方法即可。

#### 开放封闭原则 OCP

开闭原则是设计模式的总纲： 就是说我们对编码过程中的扩展是开放的，但是我们对修改是关闭的。

通俗的说就是我们写代码的过程也要做到让别人可以扩展，考虑到有扩展的场景。同时我们在改别人代码的时候也要注意只能去扩展别人的代码，而不要去随便修改别人的代码。

同样举个栗子，卖车子的场景

``` javascript
class Car {
    constructor(price) {
        this.price = price
    }

    getPrice() {
        return this.price
    }
}
```

假设现在有一个场景是要对车子的促销，~~直接传折扣进去，然后算出最终价格？~~ 现实编码中这样可能会被打。看下面的实现。

``` javascript
class Car {
    constructor(price) {
        this.price = price
    }

    getPrice() {
        return this.price
    }
}

class DiscountCar extends Car{
    constructor(price, discount){
        super(price)
        this.discount = discount
    }

    getPrice() {
        return this.price * this.discount
    }
}

const car = new DiscountCar(198000, 0.6)
console.log('car', car.getPrice())
```

如上的实现就是典型的开闭原则，我们不能去修改原来的类/方法，只可以对其进行扩展。

#### 里氏置换原则 LSP

主要是针对类的继承关系而言的，简单的说就是，父类出现的地方一定可以用子类去替换。 也就是说我们实现子类的时候，必须要实现所有的父类的方法。

**里氏替换原则的重点在不影响原功能，而不是不覆盖原方法。**

js中对类的继承等等使用的要少一些，暂不使用代码举例

#### 接口独立原则 ISP

- 使用多个专门的接口，而不使用单一的总接口。
- 客户端不应该以来其不需要的接口
- 类的依赖的建立应该在最小的接口上

不要建立庞大臃肿的接口，要尽量细化接口。但同时也要注意凡事要适度。不宜过度。

#### 依赖倒置原则 DIP

高层模块不应该依赖底层模块，二者都以更高依赖其抽象。抽象不应该依赖细节，细节要依赖抽象

通俗来说：DIP原则的本质就是通过抽象使各类或模块的实现彼此独立，互不影响，实现模块间的解藕。

还是听不懂。。。。。举个栗子

A 依赖 B，现在想改成 A 依赖 C，在实际项目中，类A一般是高层的模块，负责处理复杂的业务逻辑；类B和C是底层的模块，处理最基本的原子模块，这时候如果修改A的代码，可能会带来一些问题

**方案：** 把A的依赖修改位 interface，然后B/C各自实现接口interface，A通过interface来和B/C建立联系，降低修改A的几率

show code。。

``` javascript
// 老板想卖东西，我们Seller，总不能去一个个的去继承东西的类，这个时候去实现一个Any，然后每次去维护 any的实现即可
class Any {
    constructor(price, name) {
        this.price = price
        this.name = name
    }

    getPrice() {
        return this.price
    }

    getName() {
        return this.name
    }
}

class Seller extends Any{
    constructor(price, name) {
        super(price, name)
    }

    getPrice() {
        return this.price
    }
}
```

#### 迪米特法则

最少知道原则： 不该你知道的别知道。主要用于解藕

通俗的来说，一个类对自己的依赖的类知道的越少越好。

### 总结

由于js中没有明确的类的概念，然后函数式编程大行其道的当前，本人对于一些面向对象的思想也理解的很不到位。某些场景也没有给出代码示例。仅仅记录这个作为一篇入门文章，后续如果有深入再回来补充。

### 感悟

代码实现方法千千万，但是我们始终要考虑一个问题？
> 我们是去做一个搬砖砌墙的建筑工人，还是要做一个胸有成竹的工程师？
