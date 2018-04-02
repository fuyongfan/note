### js 实现链式调用原理
#### return this, 返回当前调用函数对象
>demo1
```
function ClassA(){
    this.prop1 = null;
    this.prop2 = null;
    this.prop3 = null;
}
ClassA.prototype = {
    method1 : function(p1){
        this.prop1 = p1;
        return this;
    },
    method2 : function(p2){
        this.prop2 = p2;
        return this;
    },
    method3 : function(p3){
        this.prop3 = p3;
        return this;
    }
};
var obj = new ClassA();

obj.method1(1).method2(2).method3(3);
```
- step1. 返回 实例 ： obj {prop1: 1}
- step2. step1的结果（obj { prop1:  1 }）调用method2 返回 obj{prop1: 1, prop2: 2}
- stop3. step2的结果（obj { prop1:  1,  prop2:  2 }）调用 method3 返回 obj { prop1:  1, prop2:  2,  prop3:  3 }

>demo2 : (1).plus(2).plus(3).minus(4)
```
Number.prototype.plus = function (val) {
    return this + val;
};
Number.prototype.minus = function (val) {
    return this - val;
};
var res = (1).plus(2).plus(3).minus(4);
console.log(res); // 2;
```
