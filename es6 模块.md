## es6 模块
### 1. export 
- export 命令规定的是对外接口， 必须与模块内部的变量建立一一对应关系
```
let a = 1;
export {a}
```
```
// 错误写法, 直接输出 1 而不是变量
let a = 1;
export a;
```
```
export function a() {
    console.log('a');
}
```
```
// 错误写法
function a() {
    console.log('a');
}
export a;
```
- 通常情况下，export输出的变量就是本来的名字，但是可以使用as关键字重命名。
```
function a() {
    console.log('b');
}
export {a as b}
```
- export命令可以出现在模块的任何位置，只要处于模块顶层就可以。如果处于块级作用域内，就会报错，
```
// 错误写法
function a() {
    console.log('abc');
    export a;
}
```
### 2. import
- import命令输入的变量都是只读的，因为它的本质是输入接口。也就是说，不允许在加载模块的脚本里面，改写接口。
```
import {a} from './main.js';
a = {};  //  Syntax Error : 'a' is read-only;
```
- 上面代码中，脚本加载了变量a，对其重新赋值就会报错，因为a是一个只读的接口。但是，如果a是一个对象，改写a的属性是允许的 —— 不建议这么做，会引起其他引用变动
- import 可以通过as 将输入的变量重命名
```
import { lastName as surname } from './profile.js';
```
- import后面的from指定模块文件的位置，可以是相对路径，也可以是绝对路径，.js后缀可以省略。如果只是模块名，不带有路径，那么必须有配置文件，告诉 JavaScript 引擎该模块的位置。
```
import {myMethod} from 'util';
```
- import命令具有提升效果，会提升到整个模块的头部，首先执行, 这种行为的本质是，import命令是编译阶段执行的，在代码运行之前。
```
foo();
import { foo } from 'my_module';
```
- import 语句会执行所加载的模块， 因此可以有下面的写法
```
import 'lodash';
```
- 多次引入只会执行依次
### 3. 模块的整体加载
- 用 * 指定一个对象， 所有输出值都加载在这个对象上面
```
// tem.js 文件
export function area(radius) {
    return Math.PI * radius * radius;
}
export function circumference(radius) {
    return 2 * Math.PI * radius;
}
```
```
// import {area, circumference} from "./tem";
import * as cycle from './tem';

console.log('圆面积: ' + circle.area(4));
console.log('圆周长: ' + circle.circumference(14))
```
- export default 就是输出一个叫做default的变量或方法，然后系统允许你为它取任意名字
- 使用 export default 命令import 时不应加 {}
```
exprot default function () {};
import fn from './tem';
```
```
// 第一组
export default function crc32() { // 输出
  // ...
}

import crc32 from 'crc32'; // 输入

// 第二组
export function crc32() { // 输出
  // ...
};

import {crc32} from 'crc32'; // 输入
```
- 如果想在一条import语句中，同时输入默认方法和其他接口，可以写成下面这样。
```
import _, { each, each as forEach } from 'lodash';
```
```
export default function (obj) {
  // ···
}

export function each(obj, iterator, context) {
  // ···
}

export { each as forEach };
```
### 4. export 和 import的复合写法
- 先输入后输出同一个模块 foo和bar实际上并没有被导入当前模块，只是相当于对外转发了这两个接口，导致当前模块不能直接使用foo和bar。
```
export { foo, bar } from 'my_module'
```
```
// 接口改名
export { foo as myFoo } from 'my_module';

// 整体输出
export * from 'my_module';
```
```
// 默认接口的写法
export { default } from 'foo'
```
```
// 没有对应的写法
import * as someIdentifier from "someModule";
import someIdentifier from "someModule";
import someIdentifier, { namedIdentifier } from "someModule";
```
### 5. 导出常量
```
export const A = 1;
export const B = 3;
export const C = 4;
```
```
import * as cycle from './tem';
console.log(cycle.A);
console.log(cycle.B);
```
