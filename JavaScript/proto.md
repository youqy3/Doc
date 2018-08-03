## prototype和__proto__

### prototype

- prototype是一个只有函数才有的属性
- 当创建函数时，JavaScript会为这个函数自动添加prototype属性，这个属性指向的是一个原型对象Functionname.prototype
- 我们可以向这个原型对象添加属性或对象，甚至可以将其指向一个现有的对象

### \_\_proto\_\_

- 每个对象都有一个\_\_proto\_\_属性，这个属性用来标识自己所继承的原型，指向继承的原型对象

### 原型链

- JavaScript通过prototype和\_\_proto\_\_在两个对象之间创建一个关联，使得一个对象可以通过委托访问另一个对象的属性和函数
- 这个关联就是原型链，一个由对象组成的有限对象链，用于实现继承和共享属性

### 原型链分析
```
function Foo(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

Foo.prototype.logName = function() {
  Foo.combineName();
  console.log(this.fullName);
}

Foo.prototype.combineName = function() {
  this.fullName = this.firstName + this.lastName;
}

var foo = new Foo('hello', 'world');
// 调用logName()时会报错，为什么？
// Uncaught TypeError: Foo.combineName is not a function
foo.logName();
```
- 根据对prototype和__proto__的分析，我们知道，实例对象的foo的__proto__属性指向构造函数Foo的prototype属性所指向的原型对象，所以foo继承了Foo.prototype对象的所有属性和方法，可以直接调用foo.combineName()（`注：`实例对象foo没有prototype属性，因为foo不是一个函数）
- 构造函数Foo的__proto__属性指向的是构造函数自己的原型对象，而不是Foo.prototype，所以Foo并不会继承Foo.prototype对象的属性和方法，直接调用Foo.combineName()会出错
- 可以通过调用Foo.prototype.combineName()或this.combineName()来解决，因为this总是指向当前实例对象，this.combineName()就相当于foo.combineName()

---

![原型链示意图](images/1)

从上图可以找出foo对象和Foo函数的原型链

```
foo.__proto__ = Foo.prototype;
foo.__proto__.__proto__ = Foo.prototype.__proto__ = Object.prototype;
foo.__proto__.__proto__.__proto__ = Foo.prototype.__proto__.__proto__ = Object.prototype.__proto__ = NULL;
```
![实例对象原型链](images/2)

```
Foo.__proto__ = Function.prototype;
Foo.__proto__.__proto__ = Function.prototype.__proto__;
Foo.__proto__.__proto__.__proto__ = Function.prototype.__proto__.__proto__ = Object.prototype.__proto__ = null;
```
![构造函数原型链](images/3)

从原型链图上可以很清晰的看出：
- 构造函数Foo的原型链上没有Foo.prototype，因此无法继承Foo.prototype上的属性和方法
- 实例对象foo的原型链上有Foo.prototype，因此foo可以继承Foo.prototype上的属性和方法

---

## 构造函数创建对象实例

### JavaScript函数有两个不同的内部方法：[[Call]]和[[Construct]]

- 如果不通过new关键字来调用函数，执行[[Call]]函数，从而直接执行代码中的函数体
- 如果通过new关键字来调用函数，则执行[[Construct]]函数，它负责创建一个实例对象
  - 实例对象创建过程：
  - 把实例对象的__proto__属性指向构造函数的prototype来实现继承构造函数prototype的所有属性和方法
  - 将this绑定到实例上
  - 然后执行函数体
```
// 根据[[Construct]]函数创建实例对象的过程来模拟一个构造函数
function createObject(proto) {
  if (!(proto === null || typeof proto === 'object' || typeof proto === 'function')) {
    throw TypeError('Argument must be an object, or null');
  }
  var obj = new Object();
  obj.__proto__ = proto;
  return obj;
}

var foo = createObject(Foo.prototype);
```
