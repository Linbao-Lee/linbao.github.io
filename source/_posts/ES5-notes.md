title: ES5 notes
date: 2018-03-22 23:30:39
alias: 学习与总结
tags: Javascript
categories:
---

## 立即执行函数
```
(function(){
    //do something
})()
```
## 理解this
 - 谁调用它，它就指向谁
 - 改变this的方法有 call, apply, bind
```
var a = 10000;
(function(){
    var p = {
        a: 10,
        test: function(){
            console.log(this.a);
        }
    }
})()
```
## 闭包 
能够读取其他函数内部变量的函数
 - 内部函数可以访问外部函数的变量
 - 可以保护内部的变量
 - 闭包可能会造成内存泄露（= null 解决）

## constructor 
构造函数和初始化一个类是一个东西。当声明一个这个类的对象的时候执行的函数就叫做构造函数

## 原型链prototype
 - 构造函数里的属性的优先级比原型链的要高
 - 面向对象编程的时候js没有类的概念，可以用函数(function a(){})代替
 - construtor实际上就是对应的那个函数本身(上句中的“a”代替)
 - prototype是按引用传递，Object.create(ABC)会创建原型ABC的副本
```
 //用js实现面向对象编程的过程
 //需求：
 // 1. 拿到父类原型链上的方法
 // 2. 不能让父类的构造函数执行两遍
 // 3. 引用的原型链不能是按址引用
 // 4. 修正子类的construtor
 var Car = function(name){
     this.name = name;
     console.log('init')
 }
 Car.prototype.sale = function(){
    console.log(name + ' has been saled')
 }
 var BMW = function(){
     Car.call(this, name);
 };
 var __obj = Object.create(Car);//如果使用new Car()，则父类的构造函数执行了两次
 __obj.constructor = BMW;
 BMW.prototype = __obj;
 var bmw = new BMW();
 console.log(bmw);
 ```
## 按值传递 与 按引用传递
 - 原型链是按引用传递的
 - 在子类继承父类时，new是把父类上的原型链给拷贝过来
 - 基本类型是按值传递，其他按引用传递

## 变量提升
```
(function(){
    console.log(a);//输出 undefined而不是ReferenceError
    var a = 1234;
    /*等同于
    var a;
    console.log(a);
    a = 1234;
    */
})()
```
另一个情形是有没有var而引进作用域的改变
```
(function(){
   var a = 10;
   var b = c = a;
})()
console.log(b);//输出10而不是ReferenceErr
/*等同于
var c;
(function(){
    var a = 10;
    c = 10;
    var b = 10;
})()
*/
```
## 函数提升（优先级比变量高）
```
(function(){
    var a = 10;
    function a(){};
    console.log(a);//输出10
})()
/*
为什么，因为函数会提升，实际是这样的:
function a(){}
var a;
q = 10;
*/
```
## 构造函数的优先级要比原型链的高
```
function test (){
    this.a = 10;
}
test.prototype.a = 20;
var q = new test;
alert(q.a);//输出10
var a = new test();
console.log(a.a)//输出10
```
