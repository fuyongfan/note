### es6 class 基本用法
- es5
```
function Fn() {
    this.x = 1;
    this.y = 2;
    this.getMsg = function () {
        return 'hello';
    }
}
Fn.prototype.sum = function () {
    return this.x + this.y;
}
```
-  转换成es6
```
class Fn {
    constructor () {
        this.x = 1;
        this.y = 2;
        this.getMsg = function () {
            return 'hello'
        }
    }
    sum () {
        return this.x + this.y;
    }
}
```
>  将es5构造函数Fn写在了construct 中，es5原型上的内容写在了 Fn 中
### 1. constructor
1.  是天生自带的属性， 生成实例的时候首先调用 constructor 方法，没写则自动添加一个空的constructor方法
```
class Fn {
    constructor () {

    }
}
class Fn {}
// 两种写法等价
```
2. constructor中return一个对象，则改变实例
```
class Fn {
    constructor () {
        return {
            a: 1,
            b: 2
        }
    }
}
var fn = new Fn();
console.log(fn instanceof Fn); // false
```
### 2. 实例
- 没有定义在this上的都定义在原型上，所有实例共享原型上的内容
- 不建议使用 __proto__修改原型
### 3. class 表达式
```
const My =  class Fn {
    constructor () {
        this.a = 1;
    }
    getName() {
        return Fn.name;
    }
}
let inst = new My();
console.log(inst.getName()); // Fn
```
- Fn 供类内部使用

### 4. 不存在变量提升
```
var fn = new Fn();
class Fn {
    constructor () {
        this.x = 1;
    }
}
console.log(fn.x); // ReferenceError: Fn is not defined
```
### 5. this 指向
```
class Fn {
    constructor () {
        this.x = 1;
        this.getMsg = this.getMsg;
    }
    getMsg () {
        return this.x + 'hello';
    }
}
var {getMsg} = new Fn();
getMsg(); // TypeError: Cannot read property 'x' of undefined
```
- 解决办法：改用箭头函数 或 使用 bind
```
class Fn {
    constructor () {
        this.x = 1;
        this.getMsg = this.getMsg.bind(this);
    }
    getMsg () {
        return this.x + 'hello';
    }
}
var {getMsg} = new Fn();
console.log(getMsg()); // '1hello'
```
```
class Fn {
    constructor () {
        this.x = 1;
        this.getMsg = () => {
            return 'hello';
        };
    }
}

var {getMsg} = new Fn();

console.log(getMsg()); // "hello"
```
### 6. 静态属性
- 定义在类上的方法， this指向类， 不是实例
- 子类可以继承静态类属性