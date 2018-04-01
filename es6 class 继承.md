## es6 class 继承
- es6 class 继承是先创建父类的实例对象this( 调用super )， 在用子类的构造函数修改this
- es5 先创建子类实例对象的this, 然后再将父类的方法添加到this上面
### 1. 概述
```
class Point {
    constructor(x, y) {
        this.x = 123;
        this.y = 456;
    }
    toString() {
        return 'red';
    }
}

class ColorPoint extends Point{
    getInfo() {
        console.log(this.x, this.y, this);
    }
}

let cp = new ColorPoint('blue', 'yellow', 'green');

cp.getInfo(); // 123 456 ColorPoint { x: 123, y: 456 }
```
- 在子类的构造函数中只有调用super之后才能使用this 关键字
```
class Point {
    constructor(x, y) {
        this.x = 123;
    }
}

class ColorPoint extends Point{
    constructor(color) {
        
    }
    getInfo() {
        console.log(this.x, this.y, this);
    }
}

let cp = new ColorPoint('blue');

// ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
```
- 子类没有添加constructor 方法时会自动添加
```
class Point {
    constructor(x, y) {
        this.x = 123;
    }
}

class ColorPoint extends Point{
    getInfo() {
        console.log(this.x, this);
    }
}

let cp = new ColorPoint('blue');
cp.getInfo(); // 123 ColorPoint { x: 123 }
```
- 子类会继承父类的静态方法 ( es5 不能 )
```
class Point {
    static sayHello() {
        return 'hello';
    }
    constructor() {
        this.x = 123;
    }
}

class ColorPoint extends Point{
    constructor() {
        super();
    }
    getInfo() {
        console.log(this.x, this);
    }
}

console.log(ColorPoint.sayHello()); // 'hello'
```
```
function Point() {
    this.x = 123;
}

Point.hello = function () {
    return 'hello';
}

function ColorPoint() {}

ColorPoint.prototype = new Point();
console.log(ColorPoint.hello()); 
// TypeError: ColorPoint.hello is not a function
```
### 2. super 关键字
>在使用方式上分为 函数调用和对象调用 两种形式：
>1. 函数调用： 只能用在子类的constructor中， 表示父类的构造函数
>2. 对象调用： 分为在静态类中调用和其他函数中调用

```
class Parent {
    constructor() {
        this.x = 1;
    }
}

class Child extends Parent {
    constructor () {
        super();
    }
    getInfo () {
        super();
    }
} 
// SyntaxError: 'super' keyword unexpected here
```
>在普通函数中调用指向父类原型
```
class A {
 p() {
    return 2;
  }
}
class B extends A {
  constructor() {
    super();
    console.log(super.p()); // 2
  }
}

let b = new B(); 
```
- super调用父类方法时，方法内部的this指向当前子类实例
```
class A {
    constructor () {
        this.x = 1;
    }
    getInfo () {
        return this.x;
    }
}

class B extends A {
    constructor () {
        super();
        this.x = 2;
        console.log(super.getInfo()); // 2
    }
}

let b = new B(); 
```
```
class A {
  constructor() {
    this.x = 1;
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
    super.x = 3;
    console.log(super.x); // undefined
    console.log(this.x); // 3
  }
}

let b = new B();
```
>在子类静态方法中调用， 指向父类的静态方法
```
class A {
    static myMethod (msg) {
        console.log(`static ${msg}`);
    }
    myMethod (msg) {
        console.log(`instance ${msg}`);
    }
}

class B extends A {
    static myMethod (msg) {
        super.myMethod(msg);
    }
    myMethod (msg) {
        super.myMethod(msg);
    }
}

B.myMethod(1);

let b = new B(); // static 1
b.myMethod(2); // instance 2
```
### 3. 实例的__proto__属性

> 子类实例的__proto__属性的__proto__属性，指向父类实例的__proto__属性。也就是说，子类的原型的原型，是父类的原型
```
var p1 = new Point(2, 3);
var p2 = new ColorPoint(2, 3, 'red');

p2.__proto__ === p1.__proto__ // false
p2.__proto__.__proto__ === p1.__proto__ // true
```