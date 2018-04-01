## Object.create()
> - 第一个参数为此对象的原型
> - 第二个参数可选，用于对对象的属性进一步描述
```
let obj = Object.create(Object.prototype, {
    x: {value: 12, writable: true, config: true, enumerable: true},
    y: {value: 34, writable: true, config: true, enumerable: true}
});

console.log(obj); // {x: 12, y: 34}
```