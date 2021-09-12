```
prototype 用于访问函数的原型对象
__proto__  / Object.getPrototypeOf() 用于访问对象实例的原型对象 
function Test() {}
const test = new Test()
test.__proto__ == Test.prototype // true
也就是说，函数拥有 prototype 属性，对象实例拥有 __proto__ 属性，它们都是用来访问原型对象的
所以函数能通过 __proto__ 访问它的原型对象。

由于 prototype 是一个对象，所以它也可以通过 __proto__ 访问它的原型对象。对象的原型对象，那自然是 Object.prototype 了。

Object.prototype.__proto__ == null // true
既然 null 是万物的终点，那使用 Object.create(null) 创建的对象是没有 __proto__ 属性的，也没有 prototype 属性。
函数有点特别，它不仅是个函数，还是个对象。所以它也有 __proto__ 属性
Object.__proto__ == Function.prototype
Function.prototype.__proto__ == Object.prototype
```