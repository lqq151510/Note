# `async`和 `await`：让异步代码写起来像同步代码

`async`和 `await`是 ES2017 引入的 JavaScript 语法，专门用于**简化 Promise 的使用**，让异步代码的书写和阅读更像同步代码，彻底告别回调地狱。

## 核心概念速览

|特性|作用|类比|
|---|---|---|
|**`async`**​|声明一个函数是异步的，会自动将返回值包装为 Promise。|给函数贴个"异步任务"标签，告诉JS："这个函数里有异步操作"。|
|**`await`**​|暂停`async`函数的执行，等待 Promise 完成，然后返回结果。|在快餐店等餐：  <br>1. 点餐（发起异步请求）  <br>2. **等待**餐做好（`await`）  <br>3. 拿到餐后继续（继续执行代码）|

## 基本用法

### 1. `async`函数

在函数声明前添加 `async`关键字，这个函数就会自动返回一个 Promise。

```
// 普通函数返回数字
function normalFunc() {
  return 42;
}
console.log(normalFunc()); // 42

// async函数返回Promise
async function asyncFunc() {
  return 42; // 实际上返回 Promise.resolve(42)
}
asyncFunc().then(result => console.log(result)); // 42
```

### 2. `await`表达式

`await`只能在 `async`函数内部使用，它会"暂停"函数的执行，直到 Promise 完成（解决或拒绝）。

```
// 模拟一个异步操作
function fetchData() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('数据加载完成');
    }, 1000);
  });
}

// 使用 async/await
async function getData() {
  console.log('开始获取数据...');
  
  // await 会等待 Promise 完成
  const result = await fetchData();
  
  console.log(result); // 1秒后执行
  console.log('获取完成');
  
  return result;
}

getData();
```

## 与传统 Promise 的对比

下表展示了从回调地狱到 `async/await`的演进：

|方式|代码示例|问题/特点|
|---|---|---|
|**回调地狱**​|`a(() => { b(() => { c(() => { ... }) }) })`|嵌套深，难以阅读和维护|
|**Promise链**​|`a().then(b).then(c).catch(...)`|链式调用，但仍有回调痕迹|
|**async/await**​|`const r1 = await a(); const r2 = await b();`|**同步写法，异步执行**，代码最清晰|

### 实际示例对比

假设我们有三个依次依赖的异步操作：

```
// 三个异步函数
function step1() {
  return new Promise(resolve => setTimeout(() => resolve('步骤1完成'), 1000));
}
function step2(data) {
  return new Promise(resolve => setTimeout(() => resolve(data + ' → 步骤2完成'), 1000));
}
function step3(data) {
  return new Promise(resolve => setTimeout(() => resolve(data + ' → 步骤3完成'), 1000));
}
```

**1. 传统 Promise 链式调用：**

```
step1()
  .then(result1 => {
    console.log(result1);
    return step2(result1);
  })
  .then(result2 => {
    console.log(result2);
    return step3(result2);
  })
  .then(result3 => {
    console.log(result3);
    console.log('全部完成');
  })
  .catch(error => {
    console.error('出错了:', error);
  });
```

**2. 使用 `async/await`：**

```
async function runAllSteps() {
  try {
    const result1 = await step1();
    console.log(result1);
    
    const result2 = await step2(result1);
    console.log(result2);
    
    const result3 = await step3(result2);
    console.log(result3);
    
    console.log('全部完成');
  } catch (error) {
    console.error('出错了:', error);
  }
}

runAllSteps();
```

可以看到，`async/await`的写法：

1. 更接近同步代码的线性结构
    
2. 更容易理解执行顺序
    
3. 错误处理可以用熟悉的 `try/catch`
    

## 错误处理

处理异步错误有三种主要方式：

### 方式1：`try...catch`（最推荐）

```
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    return data;
  } catch (error) {
    // 捕获所有错误：网络错误、JSON解析错误、业务逻辑错误等
    console.error('获取数据失败:', error);
    return null; // 返回默认值
  }
}
```

### 方式2：在调用处使用 `.catch()`

```
async function fetchData() {
  const response = await fetch('https://api.example.com/data');
  return response.json();
}

fetchData()
  .then(data => console.log(data))
  .catch(error => console.error('失败:', error));
```

### 方式3：混合使用

```
async function fetchData() {
  const response = await fetch('https://api.example.com/data').catch(error => {
    console.error('网络错误:', error);
    throw new Error('网络请求失败');
  });
  
  if (!response.ok) {
    throw new Error(`HTTP错误: ${response.status}`);
  }
  
  return response.json();
}
```

## 高级用法与技巧

### 1. 并行执行多个异步操作

当多个异步操作**没有依赖关系**时，应该并行执行以提高效率：

```
// ❌ 错误：顺序执行（慢）
async function slowWay() {
  const user = await fetchUser();  // 等2秒
  const posts = await fetchPosts(); // 再等2秒，总共4秒
  return { user, posts };
}

// ✅ 正确：并行执行（快）
async function fastWay() {
  // 同时发起两个请求
  const [user, posts] = await Promise.all([
    fetchUser(),   // 2秒
    fetchPosts()   // 2秒
  ]); // 总共只需2秒
  
  return { user, posts };
}
```

### 2. 在循环中使用 `await`

```
// 顺序处理数组（一个接一个）
async function processItems(items) {
  for (const item of items) {
    const result = await processItem(item); // 等待当前项完成
    console.log(result);
  }
}

// 并行处理数组（同时进行）
async function processItemsParallel(items) {
  const promises = items.map(item => processItem(item));
  const results = await Promise.all(promises); // 等待所有完成
  console.log(results);
}
```

### 3. 立即执行异步函数

```
// 使用 IIFE（立即调用函数表达式）
(async () => {
  const data = await fetchData();
  console.log(data);
})();

// 在模块顶层使用（ES2022+）
const data = await fetchData();
console.log(data);
```

## 在 Vue 3 中的实际应用

在 Vue 组件中，`async/await`通常与生命周期钩子或事件处理函数一起使用：

```
<script setup>
import { ref, onMounted } from 'vue';
import { fetchUserData, saveUserData } from './api';

const user = ref(null);
const loading = ref(false);
const error = ref(null);

// 组件加载时获取数据
onMounted(async () => {
  try {
    loading.value = true;
    user.value = await fetchUserData(123);
  } catch (err) {
    error.value = err.message;
  } finally {
    loading.value = false;
  }
});

// 事件处理函数
async function handleSubmit() {
  if (!user.value) return;
  
  try {
    loading.value = true;
    await saveUserData(user.value);
    alert('保存成功！');
  } catch (err) {
    alert('保存失败: ' + err.message);
  } finally {
    loading.value = false;
  }
}
</script>
```

## 常见陷阱与最佳实践

|陷阱|错误示例|正确做法|
|---|---|---|
|**忘记使用 `await`**​|`const data = fetch(url);`|`const data = await fetch(url);`|
|**在非 async 函数中用 `await`**​|`function f() { await x; }`|`async function f() { await x; }`|
|**忽略错误处理**​|`const data = await fetch()`|用 `try/catch`或 `.catch()`|
|**不必要的顺序执行**​|`await a(); await b();`(如果a、b不依赖)|`await Promise.all([a(), b()])`|
|**过度使用 async**​|对同步操作也标记 async|只有需要 await 时才用 async|

## 总结

`async/await`的本质是：

1. **语法糖**：底层仍然是 Promise，只是写法更简洁
    
2. **同步风格**：用写同步代码的方式写异步代码
    
3. **错误友好**：可以使用 `try/catch`统一处理同步和异步错误
    

记住这个简单的规则：

- **想用 `await`→ 函数前加 `async`**
    
- **函数标记了 `async`→ 返回值自动包装为 Promise**
    

这使得异步代码的编写、阅读和调试都变得更加直观，是现代 JavaScript 异步编程的首选方式。