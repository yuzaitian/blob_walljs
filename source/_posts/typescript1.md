---
title: Typescript学习（一）：高级类型
date: 2018-04-11 18:38:14
tags:
  - TypeScript
---
### 交叉类型
交叉类型就是将多个类型合并成一个新的类型，构成的这个新类型具有这几个类型的成员，拥有这几个类型的特性，就像是集合中的并集。
<!-- more -->
```typescript
function extend<T, U>(first: T, second: U): T & U {
    let result = <T & U>{};
    for (let id in first) {
        (<any>result)[id] = (<any>first)[id];
    }
    for (let id in second) {
        if (!result.hasOwnProperty(id)) {
            (<any>result)[id] = (<any>second)[id];
        }
    }
    return result;
}

class Person {
    constructor(public name: string) { }
}
interface Loggable {
    log(): void;
}
class ConsoleLogger implements Loggable {
    log() {
        // ...
    }
}
var jim = extend(new Person("Jim"), new ConsoleLogger());
var n = jim.name;
jim.log();
```
例如上面代码中，jim拥有Person中的name属性，也有myLoggable中的log()方法。

### 联合类型
联合类型就像是多个类型的交集，只有它们共有的特性才是联合类型拥有的特性。
```typescript
function padLeft(value: string, padding: any) {
    if (typeof padding === "number") {
        return Array(padding + 1).join(" ") + value;
    }
    if (typeof padding === "string") {
        return padding + value;
    }
    throw new Error(`Expected string or number, got '${padding}'.`);
}

padLeft("Hello world", 4);
```
我们可以看出上面的代码中存在一个问题，将padding参数的类型定义为any表示可以传任何值给padding，即我们可以传不是number和string类型的值给padding，Typescript编译时不会报错，只有在运行的时候才会报错。

解决这个问题，就可以使用联合类型，用竖线分割类型，如下：
```typescript
function padLeft(value: string, padding: string | number) {
    ........
}
let f = padLeft("Hello world", true);
```
### 可以为null的类型
TypeScript具有两种特殊的类型，null和undefined，它们分别具有值null和undefined。默认情况下，类型检查器认为 null与 undefined可以赋值给任何类型。 null与 undefined是所有其它类型的一个有效值。
–strictNullChecks标记可以解决此错误：当声明一个变量时，它不会自动地包含 null或 undefined。可以使用联合类型明确的包含它们:
```typescript
let s = "foo";
s = null; // 错误, 'null'不能赋值给'string'
let sn: string | null = "bar";
sn = null; // 可以

sn = undefined; // error, 'undefined'不能赋值给'string | null'
```
### 类型别名
类型别名就是给类型起一个别名，而且可以用于基础的数据类型
```typescript
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
function getName(n: NameOrResolver): Name {
    if (typeof n === 'string') {
        return n;
    }
    else {
        return n();
    }
}
```
### 类型别名与接口的不同：
1. 类型别名不会新建一个类型，只是类型的名字变了而已
2. 类型别名不能被extends和implements
### 可辨识联合
可以合并字符串字面量类型，联合类型，类型保护和类型别名来创建一个叫做可辨识联合的高级模式

先定义三个将要联合的接口，每个接口都有kind属性，但是值不同，kind属性可以作为可辨识的特征和标识
```typescript
interface Square {
    kind: "square";
    size: number;
}
interface Rectangle {
    kind: "rectangle";
    width: number;
    height: number;
}
interface Circle {
    kind: "circle";
    radius: number;
}
```
然后将他们联合到一起
```typescript
Type Shape = Square | Rectangle | Circle
```
再使用可辨识联合
```typescript
function area(s: Shape) {
    switch (s.kind) {
        case "square": return s.size * s.size;
        case "rectangle": return s.height * s.width;
        case "circle": return Math.PI * s.radius ** 2;
    }
}
```
注意：如果在Shape中添加新的类型，那么在switch下也要添加相应的判断。