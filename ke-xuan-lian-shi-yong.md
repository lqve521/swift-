# 可选链 {#h1-u53EFu9009u94FE}

### 可选连的概念 {#h3-u53EFu9009u8FDEu7684u6982u5FF5}

* 它的可选性体现于请求或调用的目标当前可能为空（nil）
  * 如果可选的目标有值，那么调用就会成功；
  * 如果选择的目标为空（nil），则这种调用将返回空（nil）
* 多次调用被链接在一起形成一个链，如果任何一个节点为空（nil）将导致整个链失效。
* 可选链的使用
  * 在可选类型后面放一个问号，可以定义一个可选链。
  * 这一点很像在可选值后面放一个叹号来强制拆得其封包内的值
    * 它们的主要的区别在于当可选值为空时可选链即刻失败
    * 然而一般的强制解析将会引发运行时错误。
  * 因为可选链的结果可能为nil,可能有值.因此它的返回值是一个可选类型.
    * 可以通过判断返回是否有值来判断是否调用成功
    * 有值,说明调用成功
    * 为nil,说明调用失败

### 可选链的示例 {#h3-u53EFu9009u94FEu7684u793Au4F8B}

* 从可选链中取值
  * 示例描述: 人\(Person\)有一个狗\(Dog\),狗\(Dog\)有一个玩具\(Toy\),玩具有价格\(price\)
  * 使用代码描述上述信息

```
// 1.定义类
class Person {
    var name : String
    var dog : Dog?
    init(name : String) {
        self.name = name
    }
}
class Dog {
    var color : UIColor
    var toy : Toy?
    init(color : UIColor) {
        self.color = color
    }
    func runing() {
        print("跑起来")
    }
}
class Toy {
    var price : Double = 0.0
}
// 2.创建对象,并且设置对象之间的关系
// 2.1.创建对象
let person = Person(name: "小明")
let dog = Dog(color: UIColor.yellow)
let toy = Toy()
toy.price = 100.0
// 2.2.设置对象之间的关系
person.dog = dog
dog.toy = toy
```

* 需求:获取
  `小明的大黄宠物的玩具价格`
  * 取出的值为可选类型,因为可选链中有一个可选类型为nil,则返回nil
  * 因此结果可能有值,可能为nil.因此是一个可选类型

```
let price = person.dog?.toy?.price
print(price) // Optional(100.0)\n
```

* 需求:给小明的大黄一个新的玩具
  * 相当于给可选类型赋值

```
person.dog?.toy = Toy()
```

* 需求:让小明的狗跑起来
  * 如果可选类型有值,则会执行该方法
  * 如果可选类型为nil,则该方法不会执行

```
person.dog?.runing()
```

## Playground

```
import UIKit

/*
 1> 从可选链中进行取值?.
 2> 给可选链进行赋值
 3> 可选链调用方法
 */


// 1.创建三个类
class Person {
    var name : String = ""
    var dog : Dog?
}

class Dog {
    var weight : Double = 0.0
    var toy : Toy?
}

class Toy {
    var price : Double = 0
    
    func flying() {
        print("飞盘在飞ing")
    }
}


// 2.创建类的对象
let p = Person()
p.name = "why"
let d = Dog()
d.weight = 60.0
let t = Toy()
t.price = 100

p.dog = d
d.toy = t


// 3.可选链的使用
// 3.1.取出why的狗的玩具的价格
/*
 该写法非常危险:
    let dog = p.dog
    let toy = dog!.toy
    let price = toy!.price
 */

/*
 该写法非常麻烦
    if let dog = p.dog {
        if let toy = dog.toy {
            let price = toy.price
        }
    }
 */
// ?.就是可选链: 系统会自动判断该可选类型是否有值
// 如果有值,则解包, 如果没有值, 则赋值为nil
// 注意: 可选链条获取的值,一定是一个可选类型
if let price = p.dog?.toy?.price { // Double/nil
    print(price)
}

let price = p.dog?.toy?.price


// 3.2.给why的狗的玩具赋值一个新的价格
// 如果可选链中有一个可选类型是没有值, 那么语句直接不执行
p.dog?.toy?.price = 50


// 3.3.可选链调用方法
/*
 该写法太麻烦
    if let dog = p.dog {
        if let toy = dog.toy {
            toy.flying()
        }
    }
 */

p.dog?.toy?.flying()
```



