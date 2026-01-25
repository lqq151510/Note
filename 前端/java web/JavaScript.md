# JavaScript 全面详解

## 📋 JavaScript 概述

**JavaScript**​ 是一种高级的、解释型的编程语言，主要用于网页交互。它是一门动态类型的、基于原型的、多范式的脚本语言，支持面向对象、命令式和函数式编程风格。

### 核心特点：

- **客户端脚本语言**：主要在浏览器中执行
    
- **解释型语言**：不需要编译，直接由 JavaScript 引擎执行
    
- **单线程**：同一时间只能执行一个任务
    
- **事件驱动**：基于事件和回调
    
- **跨平台**：所有现代浏览器都支持
    
- **与 HTML/CSS 集成**：构成 Web 开发的三大基石
    

---

## 🔧 JavaScript 基础语法

### 1. 变量声明

```
// ES5 及之前
var name = "John";  // 函数作用域

// ES6+ 新语法
let age = 25;       // 块级作用域
const PI = 3.14159; // 常量，不可重新赋值
let undefinedVar;   // 未定义，值为 undefined
let nullVar = null; // 空值
```

### 2. 数据类型

```
// 原始数据类型
let str = "Hello";           // 字符串
let num = 42;               // 数字
let bigInt = 1234567890123456789012345678901234567890n;  // BigInt
let bool = true;            // 布尔值
let undef;                  // undefined
let empty = null;           // null
let sym = Symbol("id");     // Symbol

// 引用数据类型
let obj = { name: "John", age: 30 };  // 对象
let arr = [1, 2, 3, 4, 5];           // 数组
let func = function() { return "Hello"; };  // 函数
let date = new Date();               // Date 对象
```

### 3. 类型检测与转换

```
// 类型检测
typeof "hello"           // "string"
typeof 42               // "number"
typeof true             // "boolean"
typeof undefined        // "undefined"
typeof null             // "object" (历史遗留问题)
typeof {}               // "object"
typeof []               // "object"
typeof function() {}    // "function"

// 类型转换
let num = Number("123");    // 123
let str = String(123);      // "123"
let bool = Boolean(1);      // true
let int = parseInt("10px"); // 10
let float = parseFloat("3.14"); // 3.14

// 隐式转换
"5" + 3      // "53" (字符串连接)
"5" - 3      // 2    (数字运算)
true + 1     // 2
null + 1     // 1
undefined + 1 // NaN
```

---

## 🔄 运算符

### 1. 算术运算符

```
let a = 10, b = 3;

a + b     // 13，加法
a - b     // 7，减法
a * b     // 30，乘法
a / b     // 3.333...，除法
a % b     // 1，取余
a ** b    // 1000，指数（ES7）
a++       // 后自增
++a       // 前自增
a--       // 后自减
--a       // 前自减
```

### 2. 比较运算符

```
5 == "5"    // true，值相等
5 === "5"   // false，值和类型都相等
5 != "5"    // false
5 !== "5"   // true
5 > 3       // true
5 < 3       // false
5 >= 5      // true
5 <= 4      // false
```

### 3. 逻辑运算符

```
true && false   // false，逻辑与
true || false   // true，逻辑或
!true           // false，逻辑非

// 短路求值
true && console.log("执行")  // 执行
false && console.log("不执行") // 不执行
false || console.log("执行")  // 执行
```

### 4. 赋值运算符

```
let x = 10;
x += 5;     // x = 15
x -= 3;     // x = 12
x *= 2;     // x = 24
x /= 4;     // x = 6
x %= 4;     // x = 2
x **= 3;    // x = 8
```

### 5. 其他运算符

```
// 三元运算符
let age = 18;
let status = age >= 18 ? "成人" : "未成年";

// 空值合并运算符 ?? (ES2020)
let value = null ?? "默认值";  // "默认值"
let value2 = 0 ?? "默认值";    // 0

// 可选链操作符 ?. (ES2020)
let user = { profile: { name: "John" } };
let name = user?.profile?.name;  // "John"
let city = user?.address?.city;  // undefined (不报错)
```

---

## 📊 流程控制

### 1. 条件语句

```
// if-else
let score = 85;
if (score >= 90) {
    console.log("优秀");
} else if (score >= 80) {
    console.log("良好");
} else if (score >= 60) {
    console.log("及格");
} else {
    console.log("不及格");
}

// switch
let day = 3;
switch(day) {
    case 1:
        console.log("星期一");
        break;
    case 2:
        console.log("星期二");
        break;
    case 3:
        console.log("星期三");
        break;
    default:
        console.log("其他");
}
```

### 2. 循环语句

```
// for 循环
for (let i = 0; i < 5; i++) {
    console.log(i);
}

// while 循环
let i = 0;
while (i < 5) {
    console.log(i);
    i++;
}

// do-while 循环
let j = 0;
do {
    console.log(j);
    j++;
} while (j < 5);

// for...in (遍历对象属性)
let obj = {a: 1, b: 2, c: 3};
for (let key in obj) {
    console.log(key, obj[key]);
}

// for...of (遍历可迭代对象)
let arr = [1, 2, 3];
for (let value of arr) {
    console.log(value);
}
```

### 3. 循环控制

```
// break
for (let i = 0; i < 10; i++) {
    if (i === 5) break;
    console.log(i);  // 0,1,2,3,4
}

// continue
for (let i = 0; i < 5; i++) {
    if (i === 2) continue;
    console.log(i);  // 0,1,3,4
}

// 标签
outer: for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
        if (i === 1 && j === 1) break outer;
        console.log(i, j);
    }
}
```

---

## 🎯 函数

### 1. 函数定义

```
// 函数声明
function add(a, b) {
    return a + b;
}

// 函数表达式
const multiply = function(a, b) {
    return a * b;
};

// 箭头函数 (ES6)
const divide = (a, b) => a / b;
const square = x => x * x;
const greet = () => "Hello!";
const complex = (a, b) => {
    let result = a + b;
    return result * 2;
};
```

### 2. 函数参数

```
// 默认参数
function greet(name = "Guest") {
    return `Hello, ${name}!`;
}

// 剩余参数
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}
sum(1, 2, 3, 4, 5);  // 15

// 参数解构
function printUser({name, age}) {
    console.log(`${name} is ${age} years old`);
}
printUser({name: "John", age: 30});
```

### 3. 高阶函数

```
// 函数作为参数
function calculate(a, b, operation) {
    return operation(a, b);
}

calculate(5, 3, (x, y) => x + y);  // 8
calculate(5, 3, (x, y) => x * y);  // 15

// 函数作为返回值
function createMultiplier(multiplier) {
    return function(x) {
        return x * multiplier;
    };
}

const double = createMultiplier(2);
console.log(double(5));  // 10
```

### 4. 立即执行函数表达式 (IIFE)

```
(function() {
    console.log("立即执行");
})();

// 带参数
(function(name) {
    console.log(`Hello, ${name}`);
})("John");

// 用作模块模式
const counter = (function() {
    let count = 0;
    return {
        increment: function() { count++; },
        getCount: function() { return count; }
    };
})();
```

---

## 🏗️ 对象

### 1. 对象创建

```
// 对象字面量
let person = {
    name: "John",
    age: 30,
    isStudent: false,
    hobbies: ["reading", "coding"],
    greet: function() {
        return `Hello, I'm ${this.name}`;
    }
};

// 构造函数
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.greet = function() {
        return `Hello, I'm ${this.name}`;
    };
}
let john = new Person("John", 30);

// Object.create()
let animal = {
    eat: function() {
        console.log("Eating...");
    }
};
let dog = Object.create(animal);
dog.name = "Buddy";

// ES6 Class
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    greet() {
        return `Hello, I'm ${this.name}`;
    }
}
let alice = new Person("Alice", 25);
```

### 2. 对象操作

```
let person = { name: "John", age: 30 };

// 访问属性
console.log(person.name);     // "John"
console.log(person["age"]);   // 30

// 添加/修改属性
person.job = "Developer";
person["country"] = "USA";

// 删除属性
delete person.age;

// 检查属性
console.log("name" in person);            // true
console.log(person.hasOwnProperty("job")); // true

// 遍历属性
for (let key in person) {
    console.log(key, person[key]);
}

// 获取键、值、条目
Object.keys(person);     // ["name", "job", "country"]
Object.values(person);   // ["John", "Developer", "USA"]
Object.entries(person);  // [["name", "John"], ["job", "Developer"], ...]
```

### 3. 对象方法

```
let person = {
    firstName: "John",
    lastName: "Doe",
    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    },
    set fullName(name) {
        [this.firstName, this.lastName] = name.split(" ");
    }
};

person.fullName = "Jane Smith";
console.log(person.firstName);  // "Jane"
console.log(person.lastName);   // "Smith"
```

---

## 📦 数组

### 1. 数组创建

```
// 数组字面量
let arr1 = [1, 2, 3, 4, 5];
let arr2 = ["apple", "banana", "orange"];

// 构造函数
let arr3 = new Array();        // []
let arr4 = new Array(5);       // [empty × 5]
let arr5 = new Array(1, 2, 3); // [1, 2, 3]

// Array.of() 和 Array.from()
let arr6 = Array.of(7);        // [7]
let arr7 = Array.from("hello"); // ["h", "e", "l", "l", "o"]
```

### 2. 数组方法

```
let fruits = ["apple", "banana", "orange"];

// 添加/删除
fruits.push("grape");      // 末尾添加
fruits.pop();              // 删除最后一个
fruits.unshift("kiwi");    // 开头添加
fruits.shift();            // 删除第一个

// 切片和拼接
fruits.slice(1, 3);        // ["banana", "orange"] (不修改原数组)
fruits.splice(1, 1, "mango", "peach"); // 从索引1删除1个，添加"mango","peach"

// 连接
let moreFruits = fruits.concat(["pineapple", "watermelon"]);

// 搜索
fruits.indexOf("banana");  // 返回索引，没有返回-1
fruits.includes("apple");  // true
fruits.find(fruit => fruit.startsWith("a"));  // "apple"
fruits.findIndex(fruit => fruit.length > 5);  // 返回索引

// 排序
fruits.sort();             // 按字母排序
fruits.reverse();          // 反转
```

### 3. 数组迭代

```
let numbers = [1, 2, 3, 4, 5];

// forEach
numbers.forEach(num => console.log(num));

// map
let squares = numbers.map(num => num * num);  // [1, 4, 9, 16, 25]

// filter
let evens = numbers.filter(num => num % 2 === 0);  // [2, 4]

// reduce
let sum = numbers.reduce((total, num) => total + num, 0);  // 15

// some 和 every
let hasEven = numbers.some(num => num % 2 === 0);  // true
let allEven = numbers.every(num => num % 2 === 0); // false
```

### 4. 数组解构

```
let [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first);  // 1
console.log(second); // 2
console.log(rest);   // [3, 4, 5]

// 交换变量
let a = 1, b = 2;
[a, b] = [b, a];  // a=2, b=1

// 默认值
let [x = 1, y = 2] = [5];  // x=5, y=2
```

---

## 🔄 异步编程

### 1. 回调函数

```
function fetchData(callback) {
    setTimeout(() => {
        const data = { name: "John", age: 30 };
        callback(null, data);  // 第一个参数是错误
    }, 1000);
}

fetchData((error, data) => {
    if (error) {
        console.error("Error:", error);
    } else {
        console.log("Data:", data);
    }
});
```

### 2. Promise

```
// 创建 Promise
const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
        const success = Math.random() > 0.5;
        if (success) {
            resolve("Operation succeeded");
        } else {
            reject(new Error("Operation failed"));
        }
    }, 1000);
});

// 使用 Promise
promise
    .then(result => {
        console.log("Success:", result);
        return result.toUpperCase();
    })
    .then(upperResult => {
        console.log("Uppercase:", upperResult);
    })
    .catch(error => {
        console.error("Error:", error.message);
    })
    .finally(() => {
        console.log("Operation completed");
    });

// Promise 静态方法
Promise.all([promise1, promise2, promise3])
    .then(values => console.log(values))
    .catch(error => console.error(error));

Promise.race([promise1, promise2])
    .then(value => console.log("First:", value));
```

### 3. async/await

```
async function fetchUserData() {
    try {
        const userResponse = await fetch('https://api.example.com/user');
        const user = await userResponse.json();
        
        const postsResponse = await fetch(`https://api.example.com/posts/${user.id}`);
        const posts = await postsResponse.json();
        
        return { user, posts };
    } catch (error) {
        console.error("Failed to fetch data:", error);
        throw error;
    }
}

// 使用 async 函数
fetchUserData()
    .then(data => console.log(data))
    .catch(error => console.error(error));

// 在顶层使用 await (ES2022)
const data = await fetchUserData();
```

---

## 🎨 DOM 操作

### 1. 获取元素

```
// 通过 ID
const element = document.getElementById("myId");

// 通过类名
const elements = document.getElementsByClassName("myClass");

// 通过标签名
const divs = document.getElementsByTagName("div");

// 通过选择器
const el = document.querySelector("#myId .myClass");
const allEls = document.querySelectorAll(".myClass");

// 通过属性
const links = document.querySelectorAll("a[target='_blank']");
```

### 2. 修改元素

```
const element = document.getElementById("myElement");

// 修改内容
element.textContent = "新文本内容";
element.innerHTML = "<strong>加粗文本</strong>";
element.innerText = "纯文本";

// 修改样式
element.style.color = "red";
element.style.backgroundColor = "#f0f0f0";
element.style.fontSize = "20px";
element.classList.add("active", "highlight");
element.classList.remove("inactive");
element.classList.toggle("visible");

// 修改属性
element.setAttribute("data-id", "123");
element.getAttribute("data-id");
element.removeAttribute("data-id");
element.id = "newId";
element.className = "new-class";

// 修改数据属性
element.dataset.userId = "456";
console.log(element.dataset.userId);
```

### 3. 创建和添加元素

```
// 创建新元素
const newDiv = document.createElement("div");
newDiv.textContent = "这是一个新div";

// 创建文本节点
const textNode = document.createTextNode("一些文本");

// 添加到 DOM
document.body.appendChild(newDiv);
element.appendChild(newDiv);
element.insertBefore(newDiv, referenceElement);
element.replaceChild(newDiv, oldElement);
element.removeChild(childElement);

// 克隆元素
const clonedElement = element.cloneNode(true);  // 深度克隆
```

### 4. 事件处理

```
// 添加事件监听器
const button = document.getElementById("myButton");

function handleClick(event) {
    console.log("按钮被点击了", event);
    console.log("目标元素:", event.target);
    console.log("当前元素:", event.currentTarget);
    console.log("事件类型:", event.type);
    event.preventDefault();  // 阻止默认行为
    event.stopPropagation(); // 阻止事件冒泡
}

button.addEventListener("click", handleClick);

// 移除事件监听器
button.removeEventListener("click", handleClick);

// 常用事件
button.addEventListener("dblclick", () => console.log("双击"));
input.addEventListener("focus", () => console.log("获得焦点"));
input.addEventListener("blur", () => console.log("失去焦点"));
form.addEventListener("submit", (e) => {
    e.preventDefault();
    console.log("表单提交");
});
document.addEventListener("keydown", (e) => {
    console.log("按键:", e.key, "Code:", e.code);
});
```

### 5. 事件委托

```
// 委托给父元素
const list = document.getElementById("myList");

list.addEventListener("click", function(event) {
    if (event.target.tagName === "LI") {
        console.log("点击了列表项:", event.target.textContent);
    }
});

// 使用 matches
list.addEventListener("click", function(event) {
    if (event.target.matches("li.item")) {
        console.log("点击了item");
    }
});
```

---

## 🎯 ES6+ 新特性

### 1. 解构赋值

```
// 对象解构
const person = { name: "John", age: 30, city: "New York" };
const { name, age, ...rest } = person;
console.log(name);  // "John"
console.log(rest);  // { city: "New York" }

// 重命名和解构默认值
const { name: userName, country = "USA" } = person;

// 数组解构
const colors = ["red", "green", "blue"];
const [firstColor, secondColor, ...otherColors] = colors;
```

### 2. 展开和剩余运算符

```
// 展开运算符
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];  // [1,2,3,4,5,6]

const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };
const merged = { ...obj1, ...obj2 };  // {a:1,b:2,c:3,d:4}

// 剩余参数
function sum(...numbers) {
    return numbers.reduce((acc, num) => acc + num, 0);
}
```

### 3. 模板字符串

```
const name = "John";
const age = 30;

// 多行字符串
const multiLine = `
    Hello, ${name}!
    You are ${age} years old.
`;

// 标签模板
function highlight(strings, ...values) {
    return strings.reduce((result, str, i) => 
        result + str + (values[i] ? `<mark>${values[i]}</mark>` : ''), '');
}
const result = highlight`Hello ${name}, you are ${age} years old.`;
```

### 4. 增强的对象字面量

```
const name = "John";
const age = 30;

// 属性简写
const person = { name, age };

// 方法简写
const obj = {
    greet() {
        return "Hello";
    }
};

// 计算属性名
const propName = "dynamicKey";
const dynamicObj = {
    [propName]: "value",
    [`get${propName}`]() {
        return this[propName];
    }
};
```

### 5. 模块化

```
// math.js
export const PI = 3.14159;
export function add(a, b) { return a + b; }
export default function multiply(a, b) { return a * b; }

// app.js
import multiply, { PI, add } from './math.js';
import * as math from './math.js';  // 命名空间导入
```

---

## 💡 最佳实践

### 1. 代码规范

```
// 使用严格模式
"use strict";

// 变量命名
const MAX_RETRY_COUNT = 3;  // 常量：全大写
let userCount = 0;          // 变量：小驼峰
function getUserInfo() {}    // 函数：小驼峰
class UserService {}         // 类：大驼峰

// 使用 const 和 let
const PI = 3.14159;  // 不会改变的用 const
let counter = 0;     // 会改变的用 let
```

### 2. 错误处理

```
// try-catch
try {
    const data = JSON.parse(jsonString);
    processData(data);
} catch (error) {
    console.error("解析JSON失败:", error);
    // 重新抛出或处理错误
    throw new Error("数据处理失败", { cause: error });
} finally {
    // 清理资源
    cleanup();
}

// Promise 错误处理
fetch(url)
    .then(handleResponse)
    .catch(error => {
        console.error("请求失败:", error);
        return fallbackData;
    });

// async/await 错误处理
async function process() {
    try {
        const data = await fetchData();
        return await processData(data);
    } catch (error) {
        if (error instanceof TypeError) {
            console.error("类型错误:", error);
        } else if (error instanceof RangeError) {
            console.error("范围错误:", error);
        } else {
            console.error("未知错误:", error);
        }
        return null;
    }
}
```

### 3. 性能优化

```
// 避免全局变量
(function() {
    // 模块代码
})();

// 缓存DOM查询
const container = document.getElementById("container");
// 使用 container 而不是每次都查询

// 事件节流和防抖
function debounce(func, delay) {
    let timeout;
    return function(...args) {
        clearTimeout(timeout);
        timeout = setTimeout(() => func.apply(this, args), delay);
    };
}

window.addEventListener("resize", debounce(() => {
    console.log("窗口大小改变");
}, 250));
```

---

## 📊 JavaScript 工具链

### 1. 包管理器

```
# npm
npm init -y
npm install package-name
npm install --save-dev package-name

# yarn
yarn init -y
yarn add package-name
yarn add --dev package-name
```

### 2. 模块打包器

```
// webpack.config.js
module.exports = {
    entry: './src/index.js',
    output: {
        filename: 'bundle.js',
        path: path.resolve(__dirname, 'dist')
    },
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: 'babel-loader'
            }
        ]
    }
};
```

### 3. 转译器

```
// .babelrc
{
    "presets": ["@babel/preset-env"],
    "plugins": ["@babel/plugin-proposal-class-properties"]
}
```

---

## 🎯 总结

### JavaScript 学习路径：

1. **基础语法**：变量、数据类型、运算符
    
2. **流程控制**：条件、循环
    
3. **函数**：定义、参数、作用域
    
4. **对象和数组**：创建、操作、方法
    
5. **DOM 操作**：获取、修改、事件
    
6. **异步编程**：回调、Promise、async/await
    
7. **ES6+ 新特性**：模块、类、箭头函数
    
8. **现代开发**：工具链、框架、最佳实践
    

### 核心概念：

- **作用域和闭包**：理解变量可见性
    
- **原型和继承**：JavaScript 的面向对象机制
    
- **事件循环**：理解异步执行机制
    
- **this 绑定**：掌握上下文绑定规则
    
- **类型系统**：动态类型和类型转换
    

JavaScript 是 Web 开发的核心技术，掌握它可以构建从简单网页到复杂应用的各类项目。不断实践和学习新技术，是成为优秀 JavaScript 开发者的关键。