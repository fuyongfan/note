###Object.getOwnPropertyDescriptor
- 可以获取属性描述对象, 它的第一个参数是一个对象，第二个参数是一个字符串，对应该对象的某个属性名, Object.getOwnPropertyDescriptor(obj, 'name');
- 只能用于对象自身的属性，不能用于继承的属性。
```
var person={};
//结合Object.defineProperty方法来定义属性名，以及给值
Object.defineProperty(person,"name",{
   value:"string",
   configurable:false,
   writable:true
});
var descriptor=Object.getOwnPropertyDescriptor(person,"name");
descriptor.writable=false;
console.log(descriptor);
/*
	{
	    value: 'Nicholas',
        writable: false,
	    enumerable: false,
	    configurable: false
	}
*/

```