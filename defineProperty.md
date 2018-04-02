## defineProperty
- 作用：为对象定义新属性或修改原有的属性。
- 语法：Object.defineProperty(object, propertyname, descriptor)
>object :  必须有， 目标对象
>propertyname : 必须有， 需要定义或修改的名字
>descriptor : 必须有， 对属性的描述

- 对象属性的描述有两种体现形式， 数据描述和存取器描述
####数据描述
```
var obj = {};
Object.defineProperty(obj, "name", {
    value: 12,
    writable: true,
    configurable: true,
    enumerable: true
});
```
各参数的含义
1. value  属性对应的值， 默认undefined
```
var obj = {};
Object.defineProperty(obj, "name", {});
console.log(obj.name); // undefined
Object.defineProperty(obj, "name", {
	value: 'xiaoming'
});
console.log(obj.name); // "xiaoming"
```
2. writable 属性的值是否可以被重写。设置为true可以被重写；设置为false，不能被重写。默认为false。
```
var obj = {};
Object.defineProperty(obj, "name", {
    value: 'xiaoming',
    writable: false
});
obj.name = 'honghong';
console.log(obj.name); // "xiaoming"
```
```
var obj = {};
Object.defineProperty(obj, "name", {
    value: 'xiaoming',
    writable: true
});
obj.name = 'honghong';
console.log(obj.name); // "honghong"
```
3. configurable 是否可以删除目标属性或是否可以再次修改属性的特性（writable, configurable, enumerable）
```
var obj = {};
Object.defineProperty(obj, "name", {
    value: 'xiaoming',
    writable: true,
    configurable: false
});
delete obj.name;
console.log(obj.name); // 'xiaoming'
```
```
var obj = {};
Object.defineProperty(obj, "name", {
    value: 'xiaoming',
    writable: true,
    configurable: false
});
Object.defineProperty(obj, "name", {
    value: 'xiaoming',
    writable: true,
    configurable: false,
    enumerable: true
});
console.log(obj.name); // TypeError: Cannot redefine property: name
```
4. enumerable 此属性是否可以被枚举（使用for...in或Object.keys()）
```
var obj = {};
Object.defineProperty(obj, "name", {
    value: 'xiaoming',
    writable: true,
    enumerable: false
});
for (var key in obj) {
    console.log(obj.name);
}  // 空
```
```
var obj = {};
Object.defineProperty(obj, "name", {
    value: 'xiaoming',
    writable: true,
    enumerable: true
});
for (var key in obj) {
    console.log(obj.name);
} // 'xiaoming'
```
#### 一旦使用Object.defineProperty给对象添加属性，那么如果不设置属性的特性，那么configurable、enumerable、writable这些值都为默认的false


>存取器描述
>
>注意：
当使用了getter或setter方法，不允许使用writable和value这两个属性
get或set不是必须成对出现，任写其一就可以。如果不设置方法，则get和set的默认值为undefined

```
var obj = {};
var initValue = 'hello';
Object.defineProperty(obj,"newKey",{
    get:function (){
        //当获取值的时候触发的函数
        return initValue;    
    },
    set:function (value){
        //当设置值的时候触发的函数,设置的新值通过参数value拿到
        initValue = value;
    }
});
//获取值
console.log( obj.newKey );  //hello

//设置值
obj.newKey = 'change value';

console.log( obj.newKey ); //change value
```
