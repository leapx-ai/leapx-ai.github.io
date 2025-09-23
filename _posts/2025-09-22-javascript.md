---
layout: post
title: "javascript中一些重要的事"
date: 2025-09-22 21:13:08 +0800
categories: [javascript]
tags: [javascript]
---

## 1. 作用域（Scope）

在 JavaScript 中，作用域链（Scope Chain）是决定变量和函数可访问性的一个重要概念。它决定了变量在代码中的可见性和生命周期。作用域链的机制使得 JavaScript 能够实现闭包（Closure）等强大的功能。

#### 1.1 全局作用域（Global Scope）

- 全局作用域是代码的最外层作用域。
- 在全局作用域中定义的变量和函数是全局变量和全局函数，可以在代码的任何地方访问。
- 浏览器中，全局变量是 window 对象的属性；Node.js 中，全局变量是 global 对象的属性。

#### 1.2 局部作用域（Local Scope）

- 局部作用域是在函数内部或块级作用域（{}）中定义的变量和函数。
- 在局部作用域中定义的变量和函数只能在该作用域内访问，外部无法直接访问。
- 局部作用域可以嵌套，形成作用域链。

```javascript
function outerFunction() {
  let outerVar = "I am in the outer function";

  function innerFunction() {
    let innerVar = "I am in the inner function";
    console.log(outerVar); // 可以访问外层作用域的变量
    console.log(innerVar); // 可以访问当前作用域的变量
  }

  innerFunction();
}

outerFunction();
// console.log(outerVar); // 报错：outerVar is not defined
// console.log(innerVar); // 报错：innerVar is not defined
```

#### 1.3 块级作用域

ES6 引入了块级作用域，使用 {} 定义的代码块可以形成一个局部作用域。块级作用域中的变量使用 let 或 const 声明，不能在块级作用域外部访问。

```javascript
if (true) {
  let blockVar = "I am in the block scope";
  console.log(blockVar); // 输出：I am in the block scope
}

// console.log(blockVar); // 报错：blockVar is not defined
```

#### 1.4 作用域链的形成

作用域链是通过嵌套作用域形成的。当一个函数被调用时，JavaScript 引擎会创建一个作用域链，用于查找变量和函数。作用域链的形成规则如下：

1. 每个函数都有自己的作用域。
2. 内部函数可以访问外部函数的作用域。
3. 作用域链从最内层作用域开始，逐层向外查找，直到找到全局作用域。

#### 1.5 作用域链的查找机制

当访问一个变量时，JavaScript 引擎会按照作用域链的顺序逐层向外查找，直到找到该变量或到达全局作用域。如果在全局作用域中仍未找到该变量，则会抛出 ReferenceError 错误。

#### 1.6 闭包与作用域链

闭包（Closure）是 JavaScript 中的一个重要概念，它允许函数访问其定义时所在的作用域中的变量，即使该函数在另一个作用域中被调用。

## 2. 闭包（Closure）

函数记住并访问其词法作用域的能力，即使函数在其作用域外执行。

```javascript
function outer() {
  const secret = "123";
  return function inner() {
    console.log(secret); // 访问外部作用域的变量
  };
}
const innerFn = outer();
innerFn(); // "123"（闭包保留对 secret 的引用）
```

用途：模块化、私有变量、柯里化、事件处理等。

#### 2.1 模块化：创建隔离作用域

```javascript
// 使用闭包创建计数器模块
const counterModule = (() => {
  let count = 0; // 私有变量

  // 私有方法
  function logCountChange(action) {
    console.log(`Count ${action}. Current count: ${count}`);
  }

  // 返回公共接口
  return {
    increment: function () {
      count++;
      logCountChange("incremented");
      return count;
    },
    decrement: function () {
      count--;
      logCountChange("decremented");
      return count;
    },
    getCount: function () {
      return count;
    },
    reset: function () {
      count = 0;
      console.log("Count reset to 0");
    },
  };
})();

// 使用模块
counterModule.increment(); // Count incremented. Current count: 1
counterModule.increment(); // Count incremented. Current count: 2
counterModule.decrement(); // Count decremented. Current count: 1
console.log(counterModule.getCount()); // 1
counterModule.reset(); // Count reset to 0

// 无法直接访问私有变量
console.log(counterModule.count); // undefined
```

关键点：

- 使用立即调用函数表达式(IIFE)创建封闭作用域
- 私有变量和方法无法从外部访问
- 只暴露必要的公共接口

#### 2.2 私有变量：数据封装与保护

```javascript
// 使用闭包创建具有私有变量的银行账户
function createBankAccount(initialBalance) {
  let balance = initialBalance; // 私有变量
  let transactionHistory = []; // 私有变量

  function recordTransaction(type, amount) {
    transactionHistory.push({
      type,
      amount,
      timestamp: new Date().toISOString(),
      balance: balance,
    });
  }

  return {
    deposit: function (amount) {
      if (amount <= 0) return "Invalid amount";
      balance += amount;
      recordTransaction("deposit", amount);
      return `Deposited $${amount}. New balance: $${balance}`;
    },
    withdraw: function (amount) {
      if (amount <= 0) return "Invalid amount";
      if (amount > balance) return "Insufficient funds";
      balance -= amount;
      recordTransaction("withdrawal", amount);
      return `Withdrew $${amount}. New balance: $${balance}`;
    },
    getBalance: function () {
      return balance;
    },
    getStatement: function () {
      return [...transactionHistory]; // 返回副本而不是原始引用
    },
  };
}

// 创建账户实例
const myAccount = createBankAccount(1000);
console.log(myAccount.deposit(500)); // Deposited $500. New balance: $1500
console.log(myAccount.withdraw(200)); // Withdrew $200. New balance: $1300
console.log(myAccount.getBalance()); // 1300

// 获取交易记录
console.log(myAccount.getStatement());

// 尝试直接访问私有变量
console.log(myAccount.balance); // undefined
console.log(myAccount.transactionHistory); // undefined
```

关键点：

- 封装敏感数据（余额、交易记录）
- 防止外部直接修改内部状态
- 通过受控接口访问和修改数据

#### 2.3 柯里化：函数参数复用，动态生成具有预设行为的函数

```javascript
// demo1 - 使用闭包实现柯里化
function curry(fn) {
  return function curried(...args) {
    // 如果提供的参数足够，执行原始函数
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    }
    // 否则返回一个新函数，等待剩余参数
    else {
      return function (...nextArgs) {
        return curried.apply(this, args.concat(nextArgs));
      };
    }
  };
}

// 创建通用日志函数
function logMessage(level, source, message) {
  console.log(`[${level}] [${source}]: ${message}`);
}

// 柯里化日志函数
const curriedLog = curry(logMessage);

// 创建特定日志函数
const debugLog = curriedLog("DEBUG");
const appDebugLog = debugLog("App");
const apiDebugLog = debugLog("API");

// 使用特定日志函数
appDebugLog("User logged in"); // [DEBUG] [App]: User logged in
appDebugLog("Data loaded"); // [DEBUG] [App]: Data loaded
apiDebugLog("Request sent"); // [DEBUG] [API]: Request sent

// 创建错误日志函数
const errorLog = curriedLog("ERROR")("Database");
errorLog("Connection failed"); // [ERROR] [Database]: Connection failed

// 一步到位的用法
curriedLog("WARNING", "Security", "Unauthorized access attempt");
```

```javascript
// demo2 带验证逻辑的函数

// 创建输入验证器
function createValidator(validationRules) {
  return function (input) {
    for (const rule of validationRules) {
      if (!rule.test(input)) {
        return {
          valid: false,
          message: rule.message,
        };
      }
    }
    return { valid: true };
  };
}

// 定义验证规则
const passwordRules = [
  { test: (val) => val.length >= 8, message: "密码至少 8 个字符" },
  { test: (val) => /[A-Z]/.test(val), message: "密码必须包含大写字母" },
  { test: (val) => /[0-9]/.test(val), message: "密码必须包含数字" },
];

const emailRules = [
  { test: (val) => /.+@.+\..+/.test(val), message: "请输入有效的邮箱地址" },
];

// 创建验证函数
const validatePassword = createValidator(passwordRules);
const validateEmail = createValidator(emailRules);

console.log(validatePassword("weak"));
// { valid: false, message: "密码至少 8 个字符" }

console.log(validatePassword("StrongPass1"));
// { valid: true }

console.log(validateEmail("invalid-email"));
// { valid: false, message: "请输入有效的邮箱地址" }
```

```javascript
// demo3 创建 API 请求函数
function createApiClient(baseURL, defaultHeaders = {}) {
  return async function (endpoint, method = "GET", body = null, headers = {}) {
    const url = `${baseURL}${endpoint}`;
    const requestHeaders = { ...defaultHeaders, ...headers };

    try {
      const response = await fetch(url, {
        method,
        headers: requestHeaders,
        body: body ? JSON.stringify(body) : null,
      });

      if (!response.ok) {
        throw new Error(`API请求失败: ${response.status}`);
      }

      return await response.json();
    } catch (error) {
      console.error(`API请求错误: ${error.message}`);
      throw error;
    }
  };
}

// 创建特定 API 的客户端
const jsonPlaceholderClient = createApiClient(
  "https://jsonplaceholder.typicode.com",
  {
    "Content-Type": "application/json",
  }
);

const weatherApiClient = createApiClient("https://api.weather.com/v3", {
  Authorization: "Bearer YOUR_API_KEY",
});

// 使用生成的函数
async function fetchData() {
  try {
    // 使用 JSONPlaceholder 客户端
    const users = await jsonPlaceholderClient("/users");
    console.log("用户数据:", users);

    // 使用天气API客户端
    const weather = await weatherApiClient("/current", "GET", null, {
      Location: "New York",
    });
    console.log("纽约天气:", weather);
  } catch (error) {
    console.error("数据获取失败:", error);
  }
}

fetchData();
```

```javascript

// demo4 创建带缓存的函数工厂
function createCachedFunction(fn, cacheKeyGenerator = JSON.stringify) {
const cache = new Map();

return function(...args) {
const key = cacheKeyGenerator(args);

    if (cache.has(key)) {
      console.log('从缓存获取结果');
      return cache.get(key);
    }

    console.log('计算新结果');
    const result = fn(...args);
    cache.set(key, result);
    return result;

};
}

// 创建昂贵的计算函数
function expensiveCalculation(n) {
  console.log(`执行昂贵计算 ${n}...`);
  // 模拟耗时计算
  let result = 0;
  for (let i = 0; i < n * 1000000; i++) {
    result += Math.sqrt(i);
  }
  return result;
}

// 创建带缓存的版本
const cachedCalculation = createCachedFunction(expensiveCalculation);

// 首次调用 - 计算结果
console.log(cachedCalculation(10)); // 计算并缓存

// 相同参数再次调用 - 从缓存获取
console.log(cachedCalculation(10)); // 从缓存获取

// 不同参数调用 - 重新计算
console.log(cachedCalculation(20)); // 计算并缓存
```

关键点：

- 将多参数函数转化为一系列单参数函数
- 提前固定部分参数，创建更具体的函数
- 提高代码复用性和可读性
- 使用函数组合创建更复杂的行为，而不是深度继承链

#### 2.4 闭包的核心价值总结

- 封装与隔离：创建私有空间，保护内部实现细节
- 状态保持：函数"记住"其创建时的环境
- 函数工厂：动态生成具有预设行为的函数
- 数据隐私：实现真正意义上的私有变量
- 模块化开发：构建可重用、自包含的代码单元

#### 2.5 词法作用域（静态作用域）

函数的作用域是在函数创建（定义）时确定的，而不是调用时确定的，JavaScript 不是动态作用域。

1. 创建时确定作用域链：

   - 函数在定义时会捕获其外部的词法环境（包含所有可访问的变量）
   - 这个作用域链在函数创建时就被固定下来，不会随调用位置改变

2. 调用时不影响变量查找：

   - 函数执行时查找变量，会按照创建时确定的作用域链逐级向上查找
   - 与函数被调用的位置无关

```javascript
let globalVar = "global";

function outer() {
  let outerVar = "outer";

  function inner() {
    console.log(innerVar); // 错误：innerVar 未定义（词法作用域无此变量）
    console.log(outerVar); // ✅ 输出 'outer'（来自定义时的作用域）
    console.log(globalVar); // ✅ 输出 'global'
  }

  return inner;
}

// 测试调用位置的影响
let innerVar = "global inner"; // 全局作用域的新变量
const myInner = outer();

// 在全局作用域调用 inner
myInner();
// 输出:
// Uncaught ReferenceError: innerVar is not defined
// 'outer'
// 'global'
```

- inner() 始终访问的是它创建时的作用域（outer 函数作用域 + 全局作用域）
- 即使全局作用域有 innerVar，函数仍遵循创建时的作用域链
- outerVar 的访问证明了闭包的存在（函数记住了创建时的环境）

#### 2.6 特殊说明：this 关键字

虽然作用域是静态的，但 this 的值是在调用时动态确定的：

```javascript
const obj = {
  name: "Alice",
  sayName: function () {
    console.log(this.name); // this 取决于调用方式
  },
};

obj.sayName(); // 输出 'Alice' (this=obj)

const fn = obj.sayName;
fn(); // 输出 undefined (this=全局对象或 undefined)
```

|特性 |确定时机 |是否可变 |示例|
|变量作用域链 |函数创建时 |不可变| function foo(){}
|this 值 |函数调用时 |可变| obj.method()
|闭包捕获的变量| 函数创建时| 不可变| let x=1; ()=>x;

## 3. 所有参数传递本质上都是值传递（pass by value）

在 JavaScript 中，所有参数传递本质上都是值传递（pass by value），但对于对象类型（包括数组、函数等），传递的是指向内存地址的引用值（reference value）

#### 3.1 基本类型（Primitive Types）的传递

- 值传递：传递变量的拷贝值
- 包括：string, number, boolean, null, undefined, Symbol, BigInt
- 特点：函数内修改参数 不会影响 外部变量

```javascript
function changeValue(a) {
  a = 10; // 修改的是局部拷贝
}

let num = 5;
changeValue(num);
console.log(num); // 输出 5（未改变）
```

#### 3.2 对象类型（Object Types）的传递

- 值传递引用：传递对象在内存中的地址拷贝（非对象本身）
- 包括：Object, Array, Function, Date, 等
- 特点：
  - 函数内修改对象的属性会影响原始对象（因为指向同一地址）
  - 重新赋值整个对象不会影响 原始对象（只是修改了局部拷贝的地址）

```javascript
// 案例 1：修改对象属性 → 影响原始对象
function updateProp(obj) {
  obj.value = 100; // 修改共享对象属性
}

const myObj = { value: 42 };
updateProp(myObj);
console.log(myObj.value); // 输出 100（原始对象被修改）
```

```javascript
// 案例 2：重新赋值对象 → 不影响原始对象
function replaceObj(obj) {
  obj = { value: 200 }; // 重新指向新对象
}

const anotherObj = { value: 42 };
replaceObj(anotherObj);
console.log(anotherObj.value); // 输出 42（原始对象未变）
```

- 一切皆值传递：基本类型传值，对象类型传地址值。
- 修改对象属性：相当于通过地址找到原始对象操作。
- 重新赋值对象：只是让局部变量指向新地址，不影响外部变量。

```javascript
function counter() {
  let count = 1;

  function add() {
    count++;
  }

  return { count, add };
}

const { count, add } = counter();
console.log(count);

add();
console.log(count);
```

## 4. 原型继承（Prototypal Inheritance）

在 JavaScript 中，原型链（Prototype Chain）是实现继承和共享属性的核心机制。每个对象都有一个内部属性（`[[Prototype]]`），它指向另一个对象，这个对象就是原型（prototype）。当访问一个对象的属性时，如果该对象本身没有该属性，JavaScript 引擎会沿着原型链向上查找，直到找到该属性或到达原型链的末端（null）。

#### 4.1 对象的 `[[Prototype]]` 属性

- 每个对象都有一个内部属性 `[[Prototype]]`，它指向另一个对象。
- 这个内部属性可以通过 `Object.prototype.__proto__` 或 `Object.getPrototypeOf()` 访问。

```javascript
const obj = {};
console.log(obj.__proto__); // 输出 Object.prototype
console.log(Object.getPrototypeOf(obj)); // 输出 Object.prototype
```

#### 4.2 函数的 prototype 属性

- 每个函数在创建时都会自动生成一个 prototype 属性（指向原型对象）。原型对象默认包含 constructor 属性，指回构造函数本身。
- 当通过 new 操作符调用函数时，新创建的对象的 `[[Prototype]]` 属性会被设置为该函数的 prototype 属性。

#### 4.3 原型链的形成

当通过 new 操作符调用函数时，会创建一个新对象，并将该函数的 prototype 属性赋值给新对象的 `[[Prototype]]` 属性。这样，新对象就继承了函数的 prototype 属性中的所有属性和方法。

```javascript
Child.prototype = new Parent();
// 此时：Child.prototype.**proto** === Parent.prototype
```

```javascript
function Parent() {
  this.parentProperty = "I am a parent property";
}

Parent.prototype.parentMethod = function () {
  console.log("I am a parent method");
};

function Child() {
  Parent.call(this); // 调用父类构造函数
}

Child.prototype = Object.create(Parent.prototype); // 设置 Child 的 prototype 为 Parent 的 prototype 的副本
Child.prototype.childProperty = "I am a child property";

const child = new Child();
console.log(child.parentProperty); // 输出：I am a parent property
child.parentMethod(); // 输出：I am a parent method
console.log(child.childProperty); // 输出：I am a child property
```

#### 4.4 原型链的查找机制

当访问一个对象的属性时，JavaScript 引擎会按照以下步骤查找：

1. 先在对象自身属性中查找。
2. 若未找到，则通过 proto 到其原型对象中查找。
3. 若仍未找到，继续沿原型链向上查找（`原型对象.__proto__`）。
4. 直到 Object.prototype（终点）仍未找到，返回 undefined。

```javascript
function Person(name) {
  this.name = name;
}

// 方法定义在原型上（节省内存）
Person.prototype.sayName = function () {
  console.log(this.name);
};

const alice = new Person("Alice");

// 查找过程：
alice.sayName(); // 1. alice 自身无 sayName → 2. 通过 alice.**proto** 找到 Person.prototype.sayName
alice.toString(); // 1. alice 自身无 → 2. Person.prototype 无 → 3. Person.prototype.**proto** (Object.prototype) 找到 toString
```

```text

alice (实例)
  │
  ├── name: "Alice"                   // 自身属性
  │
  └── __proto__ → Person.prototype    // 原型
        │
        ├── sayName: function()      // 原型方法
        ├── constructor: Person      // 指回构造函数
        │
        └── __proto__ → Object.prototype
              ├── toString()         // 内置方法
              └── __proto__ → null   // 终点
```

#### 4.5 原型链的实际应用

继承

- 原型链是 JavaScript 中实现继承的主要方式。
- 通过设置子类的 prototype 属性为父类的实例，子类可以继承父类的属性和方法。

```javascript
function Animal() {
  this.species = "Animal";
}

Animal.prototype.getSpecies = function () {
  return this.species;
};

function Dog() {
  Animal.call(this);
  this.name = "Buddy";
}

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.getName = function () {
  return this.name;
};

const dog = new Dog();
console.log(dog.getSpecies()); // 输出：Animal
console.log(dog.getName()); // 输出：Buddy
```

共享属性和方法

- 原型链允许多个对象共享相同的属性和方法，从而节省内存。

```javascript
function MyClass() {}
MyClass.prototype.sharedMethod = function () {
  console.log("Shared method");
};

const obj1 = new MyClass();
const obj2 = new MyClass();

obj1.sharedMethod(); // 输出：Shared method
obj2.sharedMethod(); // 输出：Shared method
```

关键方法

1. Object.create(proto)
   创建一个新对象，并指定其原型（`__proto__` 指向传入的 proto）。
2. Object.getPrototypeOf(obj)
   返回对象的原型（替代 `obj.__proto__`）。
3. obj.hasOwnProperty(prop)
   判断属性是否为对象自身（非原型链继承）的属性。
4. prop in obj
   判断属性是否在对象或其原型链上。

## 5. JavaScript 中 this 的动态绑定

this 是 JavaScript 中最令人困惑的概念之一，因为它的值在运行时动态确定，而不是在定义时静态绑定。这种动态特性使得 this 在不同上下文中可以指向不同的对象。

#### 5.1 默认绑定（独立函数调用）

当函数独立调用（不作为对象方法）时，this 指向全局对象（浏览器中是 window，Node.js 中是 global）。在严格模式下，this 为 undefined。

```javascript
function showThis() {
  console.log(this);
}

// 非严格模式
showThis(); // window (浏览器中)

// 严格模式
function strictShowThis() {
  "use strict";
  console.log(this);
}
strictShowThis(); // undefined
```

#### 5.2 隐式绑定（方法调用）

当函数作为对象的方法被调用时，this 指向调用该方法的对象。

```javascript
const user = {
  name: "Alice",
  greet: function () {
    console.log(`Hello, ${this.name}!`);
  },
};

user.greet(); // Hello, Alice! (this = user)
```

#### 5.3 显式绑定（call, apply, bind）

使用 call(), apply() 或 bind() 方法可以显式设置 this 的值。

```javascript
function introduce(lang) {
  console.log(`My name is ${this.name}, I code in ${lang}`);
}

const person1 = { name: "Alice" };
const person2 = { name: "Bob" };

// 使用 call
introduce.call(person1, "JavaScript");
// My name is Alice, I code in JavaScript

// 使用 apply
introduce.apply(person2, ["Python"]);
// My name is Bob, I code in Python

// 使用 bind (创建新函数)
const bobIntro = introduce.bind(person2, "Java");
bobIntro();
// My name is Bob, I code in Java
```

#### 5.4 new 绑定（构造函数）

当使用 new 关键字调用函数时，this 指向新创建的对象。

```javascript
function Person(name) {
  this.name = name;
  this.greet = function () {
    console.log(`Hello, I'm ${this.name}`);
  };
}

const alice = new Person("Alice");
alice.greet(); // Hello, I'm Alice
```

#### 5.5 箭头函数绑定

箭头函数不绑定自己的 this，而是继承外层作用域的 this 值（词法作用域）。

```javascript
const timer = {
  seconds: 10,
  start() {
    // 传统函数 - this 在运行时确定
    setTimeout(function () {
      console.log(this.seconds); // undefined (this = window)
    }, 1000);

    // 箭头函数 - 继承外层 this
    setTimeout(() => {
      console.log(this.seconds); // 10 (this = timer)
    }, 1000);
  },
};

timer.start();
```

#### 5.6 this 绑定的特殊场景

```javascript
const counter = {
  count: 0,
  increment() {
    console.log(this); // 原始指向 counter

    // 问题：回调函数丢失 this 绑定
    setTimeout(function () {
      console.log(this); // window 或 undefined
      this.count++; // 错误！
    }, 100);

    // 解决方案1：箭头函数
    setTimeout(() => {
      this.count++; // 正确
    }, 100);

    // 解决方案2：bind
    setTimeout(
      function () {
        this.count++; // 正确
      }.bind(this),
      100
    );

    // 解决方案3：保存 this 引用
    const self = this;
    setTimeout(function () {
      self.count++; // 正确
    }, 100);
  },
};
```

#### 5.7 DOM 事件处理函数中的 this

在 DOM 事件处理函数中，this 指向触发事件的元素：

```html
<button id="myButton">Click me</button>

<script>
  document.getElementById("myButton").addEventListener("click", function () {
    console.log(this); // <button> 元素
  });
</script>
```

#### 5.8 类中的 this

在类中，方法中的 this 指向实例对象，但需要注意方法提取后的绑定问题：

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  // 类方法 - this 绑定实例
  greet() {
    console.log(`Hello, ${this.name}`);
  }

  // 类字段箭头函数 - this 自动绑定实例
  greetArrow = () => {
    console.log(`Hello from arrow, ${this.name}`);
  };
}

const alice = new User("Alice");
alice.greet(); // Hello, Alice

// 方法提取后丢失 this 绑定
const greet = alice.greet;
greet(); // 错误：无法读取 undefined 的 name

// 箭头函数保持 this 绑定
const greetArrow = alice.greetArrow;
greetArrow(); // Hello from arrow, Alice
```

## 6. 异步编程（事件循环）

JavaScript 作为一门单线程语言，通过事件循环（Event Loop） 机制实现了高效的异步编程能力。这种机制使得 JavaScript 在等待 I/O 操作时不会阻塞主线程，从而能够处理大量并发操作。

#### 6.1 执行环境组件

1. 堆（Heap）：存储对象和变量的内存区域
2. 调用栈（Call Stack）：LIFO，只有栈顶的函数正在执行。
3. 微任务队列（Microtask Queue）：FIFO，Promise.then / queueMicrotask / MutationObserver。
4. 宏任务队列（Macrotask Queue）：也叫 Task Queue，FIFO，setTimeout / setInterval / I/O / UI 事件 / postMessage / MessageChannel / setImmediate（Node）…

#### 6.2 事件循环运行机制

1.  执行同步代码

    a. 浏览器把 `<script>`、内联代码、模块代码一股脑压栈执行。

    b. 过程中如果遇到：

         - 同步函数 → 继续压栈
         - 微任务 API → 把回调塞进微任务队列
         - 宏任务 API → 把回调塞进宏任务队列

    c. 同步阶段不会出队任何微/宏任务。

    ```javascript
    console.log(1); // 同步
    Promise.resolve().then(() => console.log(2)); // 微任务
    setTimeout(() => console.log(3), 0); // 宏任务
    console.log(4);
    ```

    调用栈：

          [global] -> console.log(1) -> console.log(4) -> [empty]
          微任务队列： [ () => console.log(2) ]
          宏任务队列： [ () => console.log(3) ]
          输出到此为止：1 4

2.  处理微任务

    a. 当调用栈变空（即同步代码跑完）时，事件循环会一次性清空整个微任务队列。

    b. 清空期间如果又产生新的微任务，会继续追加到同一队列尾部，直到队列为空为止 —— 这意味着微任务可以“饿死”渲染。

    继续上面的例子：

          微任务队列： [ () => console.log(2) ]
          执行后输出：2
          微任务队列：[]

3.  渲染更新（浏览器专属）

          a. 浏览器检查「是否需要重排/重绘」。
          b. 如果有 requestAnimationFrame 回调，会在这时按顺序执行（不算宏任务也不算微任务，属于渲染阶段）。
          c. 执行完后，浏览器把最终像素刷到屏幕。
          d. 如果页面最小化、隐藏或刷新频率被限制，这一步可能跳过。

4.  执行一个宏任务

          a. 事件循环从宏任务队列里取一个（只取一个！）压栈执行。
          b. 执行完后回到第 1 步(下一次 tick)，重新走一遍「同步 → 微任务 → 渲染 → 宏任务」。

5.  简单总结：

          1. 执行同步代码：所有同步任务进入调用栈执行
          2. 处理微任务：当调用栈为空时，清空所有微任务
          3. 渲染更新：执行 UI 渲染（如果需要）
          4. 执行宏任务：从宏任务队列取出一个任务执行
          5. 重复循环：重复步骤 1-4

    在浏览器事件循环里，最外层的 `<script>` 标签（或 devtools snippet、import 进来的模块） 会被包装成一个「任务（task）」，然后送进宏任务队列。

    Event Loop 取到这个任务 → 压栈 → 执行整段同步代码 → 清微任务 → 可能渲染 → 结束这一帧。

    1~3 步属于当前这一轮事件循环：先跑同步代码（其实就是“当前宏任务”本身），再清微任务，再决定是否渲染。

    第 4 步「从宏任务队列取出一个任务执行」是下一轮循环的起点，因此 setTimeout、setInterval、I/O 完成 等宏任务都会在下一次事件循环中被处理。


6. 伪代码演示执行逻辑

```javascript
loop forever {
  task = macrotaskQueue.dequeue() // ← 取任务
  execute(task) // ← 把任务压栈执行（同步 JS）
  while (microtaskQueue not empty)
    microtaskQueue.dequeue()() // ← 清空微任务
  if (shouldRender())
    updateTheRendering() // ← 样式/布局/绘制/上屏
}

```

-- end --
