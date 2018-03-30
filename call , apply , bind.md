###  call , apply , bind 
> 共同点：
1. 都用来改变this指向
2. 第一个参数代表this指向

>不同点：
1. call 和 apply 立即执行， bind 返回一个新函数调用时执行
2. call 从第二个参数开始， 依次传入执行函数
3. apply 第二个参数以数组的形式传入执行函数
4. bind 从第二个参数开始，依次出入执行函数，将参数暂存( 柯理化函数 )

```
function sum(a, b, c) {
    return a + b + c;
}

var res = sum.call(null, 1, 2, 3);

console.log(res); // 6
```
```
function sum(a, b, c) {
    return a + b + c;
}

var res = sum.apply(null, [1, 2, 3]);

console.log(res); // 6
```
```
function sum(a, b, c) {
    return a + b + c;
}

var res = sum.bind(null, 1, 2);

console.log(res(3)); // 6
```