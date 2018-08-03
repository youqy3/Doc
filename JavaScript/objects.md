## Object基本概念

- Object是属性的无序集合，每个属性都是一个名/值对，属性名是字符串，可以把Object看成是字符串到值的映射

```
// Object属性名是字符串，所以定义属性名时有两种方式
// 属性名可以是包含空字符串在内的任意字符串
// 这两种方式定义得到的结果是一样的，推荐用第一种
var obj = {
  name: 'ryan',
  age: 20
};

var obj1 = {
  'name': 'ryan',
  'age': 20
};
```

- Object不仅仅是字符串到值的映射，除了可以保持自身的属性，还可以从一个称为原型的对象继承属性
- Object的方法通常是继承的属性
- Object是可变的，通过引用而非值来操作对象

## Object的操作

- Object最常见的用法是创建（create）、设置（set）、查找（query）、删除（delete）、检测（test）和枚举（enumerate）属性
  1. 对象的创建（create）
    - 可通过对象直接量、关键字new和Object.create()方法创建一个对象
    - 对象直接量是一个表达式，这个表达式的每次运算都创建并初始化一个新的对象。每次计算对象直接量的时候，也都会计算它的每个属性的值。即，如果在一个重复调用的函数中的循环体中使用了对象直接量，它将创建很多新对象，并且每次创建的对象的属性值也有可能不同
    - 关键字new：var obj = new Object(); // 等价于obj = {}
    - Object.create()是ES5新定义的创建对象的函数，Object.create()接受2个参数，第一个为参数是这个对象的原型，第二个参数可选，用于对对象的属性做进一步描述

    ```
    // Object.create()是一个静态函数，而不是提供给某个对象调用的方法
    var obj1 = Object.create({x:1, y:2}); // obj1继承了属性x和y

    // 通过传入Object.prototype创建一个空对象{}
    var obj2 = Object.create(Object.prototype);
    ```

  2. 删除属性
      - delete只是断开属性和宿主对象的联系，真正的删真正的删除操作在执行垃圾回收时
      - delete运算符只能删除自有属性，不能删除继承属性（要删除继承属性必须从定义这个属性的原型对象删除，这会影响到所有继承自这个原型的对象）
      - delete不能删除可配置性为false的属性

  3. 检测属性
    - 可以通过in运算符、hasOwnProperty()、propertyIsEnumerable()方法判断某个属性是否存在于某个对象中
    - in运算符不仅会检测自有属性，也会检测继承属性
    - hasOwnProperty()方法用于检测对象的自有属性
    - propertyIsEnumberable()是hasOwnProperty()方法的增强，只有当检测到属性为对象的自有属性且该属性的可枚举为true时才返回true

    ```
    var obj = { x: 1 };
    "x" in obj;   // true
    "y" in obj;   // false
    "toString" in obj;   // true, toString为继承属性

    obj.hasOwnProperty("x");  // true
    obj.hasOwnProperty("y");  // false
    obj.hasOwnProperty("toString")  // false
    ```
  4. 枚举属性
    - for/in循环可以遍历对象中所有可枚举的属性
    - Object.keys()返回一个数组，这个数组由对象中可枚举的自有属性的名称组成
    - Object.getOwnPropertyNames()与Object.keys()类似，只是它返回对象的所有自有属性的名称，而不仅仅是可枚举的属性

    ```
    var obj = {
      name: 'ryan',
      age: 20
    };
    Object.property.sayName = function() {
      console.log(this.name);
    };
    // 输出为name、age、sayName
    // toString、indexOf等属性由于其不可枚举，不在输出列表
    for (key in obj) {
      console.log(key);
    }
    // 输出["name", "age"]
    // 由于sayName属性是在prototype定义的，不在输出数组
    Object.keys(obj)
    // 输出["name", "age"]
    Object.getOwnPropertyNames(obj);    
    ```

- 除了名字和值之外，每个属性还有一些与之相关的值，称为“属性特性”
  - 可写（writable attribute），表明是否可以设置改属性的值
  - 可枚举（enumerable attribute），表明是否可以通过for/in循环返回该属性值
  - 可配置（configurable attribute），表明是否可以修改或删除改属性
- 除了包含属性之外，每个对象还拥有三个相关的对象特性（object attribute）
  - 对象的原型（prototype）指向另外一个对象，本对象的属性继承自它的原型对象
  - 对象的类（class）是一个标识对象类型的字符串
  - 对象的扩展标记（extensible flag）指明了是否可以向该对象添加新属性

## 属性getter和setter

- 对象属性是由名字、值和一组特性构成
- 在ES5中，属性值可以用一个或两个方法代替，这两个方法就是getter和setter
- 由getter和setter定义的属性称作“存取器属性”，它不同于“数据属性”，数据属性只有一个简单的值
- 和数据属性不同，存取器属性不具有可写性，如果属性同时具有getter和setter方法，它是一个读/写属性；只有getter方法的属性为只读属性，只有setter方法的属性为只写属性，读取只写属性总是返回undefined
- 和数据属性一样，存取器属性是可以继承的

```
var p = {
  x: 1.0,
  y: 1.0,   // x, y是普通的可读写的数据属性

  // r是可读写属性，它有getter和setter
  get r() {
    return Math.sqrt(this.x * this.x + this.y * this.y);
  },
  set r(newValue) {
      var oldValue = Math.sqrt(this.x * this.x + this.y * this.y);
      var ratio = newValue / oldValue;
      this.x *= ratio;
      this.y *= ratio;
  }

  // theta是只读属性，它只有getter方法
  get theta() {
    return Math.atan2(this.y, this.x);
  }
}
```

## 属性的特性

- 除了名字和值之外，属性还包含一些标识它们可写、可枚举和可配置的特性，其中值也可看做是属性的一种特性
- 数据属性的4个特性：值(value)、可写性(writable)、可枚举性(enumerable)和可配置性(configurable)
- 存取器属性的4个特性：读取(get)、写入(set)、可枚举性和可配置性
- 为了实现属性特性的查询和设置操作，ES5定义了一个名为“属性描述符”的对象，这个对象代表上述描述的4个特性。描述符对象的属性和它们所描述属性特性是同名的。因此，数据属性的描述符对象的属性有value、writable、enumerable和configurable。存取器属性的描述符对象则用get属性和set属性代替value和writable。其中writable、enumerable和configurable都是布尔值，get和set是函数值。
- Object.getOwnPropertyDescriptor()方法用于获取某个对象特定属性的属性描述符，Object.getOwnPropertyDescriptor()只能获取自有属性的描述符，对继承属性和不存在的属性，返回undefined

```
var obj = {
  name: 'ryan',
  age:26
};
// 输出为{value: "rayn", writable: true, enumerable: true, configurable: true}
Object.getOwnPropertyDescriptor(obj, 'name');
```
- 要想设置属性的特性，或者想让新建属性具有某种特性，需要调用Object.defineProperty()，传入的参数为要修改的对象，要创建或修改的属性名及属性描述符，Object.defineProperty()只能修改已有属性或新建自有属性，但不能修改继承属性
- Object.definePropertys()用于修改多个属性的特性

```
var obj = {};
// 添加一个不可枚举的数据属性x，并赋值为1  
Object.defineProperty(obj, "x", {
  value: 1,
  writable: true,
  enumerable: false,
  configurable: true
  });

// 属性是存在的，但不可枚举    
obj.x;  // => 1
Object.keys(obj);  // => []

// 对属性x做修改，让它变为只读
Object.defineProperty(obj, "x", {
  writable: false
  });

// 试图更改这个属性的值
obj.x = 2;  // 操作失败但不报错，在严格模式下抛出类型错误异常

obj.x;  // => 1

// 属性依然是可配置的，因此可以用这种方式对它进行修改
Object.defineProperty(obj, "x", {
  value: 2
  });

obj.x;  // => 2

// 现将x从数据属性修改为存取器属性
Object.defineProperty(obj, "x", {
  get: function() {return 0;}
  });

obj.x;  // => 0

// 对于用Object.defineProperty()新建的属性，默认的特性值是false或undefined
// 直接定义的属性，默认的特性值为true
var o = {name: 'ryan'};
Object.defineProperty(o, "age", {
  value: 1
  });

// 输出为{value: "ryan", writable: true, enumerable: true, configurable: true}
Object.getOwnPropertyDescriptor(o, 'name');

// 输出为{value: 1, writable: false, enumerable: false, configurable: false}
Object.getOwnPropertyDescriptor(o, 'age');  

// Object.definePropertys定义对象的多个属性描述
Object.definePropertys(o, {
  x: {value: 1},
  y: {value: 2}
  });       
```
- 描述符对象修改规则：
  1. 如果对象是不可扩展的，则可以编辑已有的自有属性，但不能给它添加新属性
  2. 如果属性是不可配置的，则不能修改它的可配置性和可枚举性
  3. 如果存取器属性是不可配置的，则不能修改其getter和setter方法，也不能将它转换为数据属性
  4. 如果数据属性是不可配置的，则不能将它转换为存取器属性
  5. 如果数据属性是不可配置的，则不能将它的可写性从false改为true，但可以从true改为false
  6. 如果数据属性是不可配置且不可写的，则不能修改它的值，然而可配置但不可写属性的值是可以修改的

## 原型属性

- Object.getPrototypeOf(objName)用于查询对象的原型
- p.isPrototypeOf(o)用来检测p是否是o的原型

## 对象的可扩展性

- 对象的可扩展性用以表示是否可以给对象添加新属性
- 所有内置对象和自定义对象都是显示可扩展的，宿主对象的可扩展性是由JS引擎定义的
- Object.isExtensible(objName)用于判断该对象是否是可扩展的
- Object.preventExtensibles()，将待转换的对象作为参数传进去
- 一旦将对象转换为不可扩展的，就无法再将其转换为可扩展的了
- preventExtensibles只影响到对象本身的可扩展性
- Object.seal()和Object.preventExtensibles()类似，除了能将对象设置为不可扩展的，还可以将对象的所有自身属性都设置为不可配置的，但已有的可写属性依然可以设置，可用Object.isSealed()来检测对象是否封闭
- Object.freeze()将更严格地锁定对象 - “冻结”，除了可以将对象设置为不可扩展的和将其属性设置为不可配置的之外，还可以将它自有的所有数据属性设置为只读（如果对象的存取器属性具有setter方法，存取器属性将不收影响，仍然可以通过属性赋值调用它们），可用Object.isFrozen()来检测对象是否冻结。
- Object.preventExtensibles()、Object.seal()和Object.freeze()都返回传入的对象，即可以通过函数嵌套的方式调用它们