title: JS-对象
date: 2018-03-23 00:07:39
alias:
tags: Javascript
categories:
---
## 三类对象两大属性：
- 内置对象  Array Number Math Date Regex
- 宿主对象  Windows Document 
- 自定义对象 
 -- 自有属性
 -- 继承属性

## 对象的创建
 - 对象直接量   var obj = {}; obj.x = 1;...
 - 通过new创建  var obj = new Obj();  //通过构造函数
 - 通过Object.create创建 //普通对象都有原型，Object.prototype没有.new Date()同时继承Date.prototype和Object.prototype
 -- Object.create是创建一个新的对象，有三个参数，其中第一个参数是传入这个对象的原型(表明这个新对象是在优先惹出上创建的) var o = Object.create({x:1,y:2});
 -- 可通过传入一个null来创建一个没有原型的对象
 -- 代码示例：通过原型继承创建一个新对象，因为ECMAScript3没有create方法，所以要用代码加以区别：

```javascript
        function inherit(p){
            if(null == p) throw TypeError();
            if(Object.create) return Object.create(p);
            var t = typeof p;
            if(t != 'function' || t != 'object' ) throw TypeError();
            function f(){};
            f.prototype = p;
            return new f();
        }
```
## 对象属性的查询和设置
 -- 对象都是关联数组
 -- book.title      //正确
 -- book[title]    //正确
 -- 可以用for in循环遍历属性
## 继承
 -- 在javascript中只有在查询的时候才会体会到继承的存在，而设置属性则和继承无关
 -- 如果用上面inherit继承（标准继承）则会出现同名属性覆盖
 -- 属性访问出错的一个安全方法: var len = book && book.subtitle && book.subtitle.length;
##删除属性
 -- 用delete删除对象的属性
 -- 只能删除自有属性，不能删除继承属性
 -- delete只是断开属性和宿主对象之间的关系
##检测属性
 -- in //自有属性&继承属性含有，则返回true // var obj = {x:1,y:2}; x in obj => true // 可区分属性存在且值为undefined的属性=>返回false
 -- hasOwnProperty()方法 // 对自有属性返回true，对继承属性返回false
 -- propertyIsEnumerable() 方法 //是自有属性且这个属性是可枚举的 => 方法不可枚举
 -- 获得可枚举的属性的方法有两个：Object.keys() 和 Object.getOwnPropertyNames()
## 属性的getter和setter方法（存取器属性）
 -- 可以定义可读可写属性
 -- 可以被继承
 -- 在函数体内this表示这个点的对象
 -- 示例：

 ```javascript
    var p = {
        x : 1,
        y : 2,
        get r(){return this.x * this.y;};
        set  r(value){this.x += value; this.y -= value;}, //r是可读可写属性
        get s(){ return Math.atan2(this.x,this.y);} // s是只读属性
    }
```

## 对象的特性

  -- 通过对象的直接量创建的对象使用Object.prototype作为它的原型
  -- 通过new创建的对象使用构造函数作为它的原型
  -- 通过Object.create创建的对象使用它的第一个对象作为它的原型
  以是三点很重要 
此外，对象还有
    -- 类属性  通过toString打印出来的是 [object class]
    -- 可序列化  用JSON.stringify()来序列化，JSON.parse()来还原
