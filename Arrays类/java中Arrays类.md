# Java Arrays 类常用方法详解

## 📊 方法概览

|方法名|功能描述|时间复杂度|注意事项|
|---|---|---|---|
|`toString()`|将数组转换为字符串|O(n)|方便调试输出|
|`binarySearch()`|二分查找元素|O(log n)|数组必须已排序|
|`copyOf()`|拷贝指定长度的数组|O(n)|可扩容或截断|
|`copyOfRange()`|拷贝指定范围的数组|O(n)|包含起始索引，不包含结束索引|
|`fill()`|填充数组元素|O(n)|用相同值填充整个数组|
|`sort()`|数组排序|O(n log n)|使用快速排序算法|

---

## 🔍 方法详细说明

### 1. `toString()`方法

**功能**: 将数组转换为字符串格式，便于输出和调试

**语法**:

```
static String toString(数据类型[] a)
```

**示例**:

```
int[] arr = {1, 2, 3, 4, 5};
System.out.println(Arrays.toString(arr));  // 输出: [1, 2, 3, 4, 5]
```

**输出格式**: `[元素1, 元素2, 元素3, ...]`

---

### 2. `binarySearch()`方法

**功能**: 在已排序的数组中执行二分查找

**语法**:

```
static int binarySearch(数据类型[] a, 数据类型 key)
```

**参数**:

- `a`: 已排序的数组
    
- `key`: 要查找的值
    

**返回值**:

- 找到: 返回元素的索引
    
- 未找到: 返回负值（插入点位置）
    

**示例**:

```
int[] arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
int index = Arrays.binarySearch(arr, 5);  // 返回: 4
```

**注意事项**:

- 数组必须已排序，否则结果不可预测
    
- 如果数组包含多个相同的元素，不保证返回哪个索引
    

---

### 3. `copyOf()`方法

**功能**: 拷贝原数组，指定新数组的长度

**语法**:

```
static 数据类型[] copyOf(数据类型[] original, int newLength)
```

**参数**:

- `original`: 原数组
    
- `newLength`: 新数组的长度
    

**特性**:

- 如果 `newLength > original.length`: 多余位置填充默认值
    
- 如果 `newLength < original.length`: 截断原数组
    
- 深拷贝：新数组与原数组是独立的对象
    

**示例**:

```
int[] arr1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
int[] arr2 = Arrays.copyOf(arr1, 5);  // 结果: [1, 2, 3, 4, 5]
```

---

### 4. `copyOfRange()`方法

**功能**: 拷贝原数组的指定范围

**语法**:

```
static 数据类型[] copyOfRange(数据类型[] original, int from, int to)
```

**参数**:

- `original`: 原数组
    
- `from`: 起始索引（包含）
    
- `to`: 结束索引（不包含）
    

**范围说明**: 包含起始索引，不包含结束索引（左闭右开区间）

**示例**:

```
int[] arr1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
int[] arr3 = Arrays.copyOfRange(arr1, 2, 5);  // 结果: [3, 4, 5]
```

---

### 5. `fill()`方法

**功能**: 用指定值填充整个数组的所有元素

**语法**:

```
static void fill(数据类型[] a, 数据类型 val)
```

**参数**:

- `a`: 要填充的数组
    
- `val`: 填充值
    

**示例**:

```
int[] arr4 = new int[5];
Arrays.fill(arr4, 10);  // 结果: [10, 10, 10, 10, 10]
```

**变体方法**:

- `fill(数据类型[] a, int fromIndex, int toIndex, 数据类型 val)`: 填充指定范围
    

---

## 🔧 Comparator 自定义排序

### 6. `sort()`方法 - 自定义比较器

**功能**: 对数组进行排序，支持自定义比较规则

**基本语法**:

```
// 匿名内部类方式
Arrays.sort(array, new Comparator<T>() {
    @Override
    public int compare(T o1, T o2) {
        // 自定义比较逻辑
        return o1 - o2;  // 升序
        // return o2 - o1;  // 降序
    }
});

// Lambda 表达式方式（Java 8+）
Arrays.sort(array, (o1, o2) -> o1 - o2);
```

**比较规则说明**:

|返回值|含义|排序结果|
|---|---|---|
|负数|o1 < o2|o1 排在 o2 前面|
|0|o1 == o2|顺序不变|
|正数|o1 > o2|o1 排在 o2 后面|

**示例**:

```
System.out.println("------自定义排序：升序------");
Integer[] arr1 = {5, 2, 8, 1, 9, 3};
Arrays.sort(arr1, new Comparator<Integer>() {
    @Override
    public int compare(Integer o1, Integer o2) {
        // 升序：o1 - o2
        return o1 - o2;
    }
});
System.out.println("升序结果: " + Arrays.toString(arr1));
// 输出: [1, 2, 3, 5, 8, 9]

System.out.println("------自定义排序：降序------");
Integer[] arr2 = {5, 2, 8, 1, 9, 3};
Arrays.sort(arr2, new Comparator<Integer>() {
    @Override
    public int compare(Integer o1, Integer o2) {
        // 降序：o2 - o1
        return o2 - o1;
    }
});
System.out.println("降序结果: " + Arrays.toString(arr2));
// 输出: [9, 8, 5, 3, 2, 1]
```

**Lambda 表达式方式**:

```
Integer[] arr3 = {5, 2, 8, 1, 9, 3};
// 升序
Arrays.sort(arr3, (a, b) -> a - b);
System.out.println("Lambda 升序: " + Arrays.toString(arr3));

// 降序
Arrays.sort(arr3, (a, b) -> b - a);
System.out.println("Lambda 降序: " + Arrays.toString(arr3));
```

---

## 🎯 常用比较模式

### 1. 基本类型包装类排序

```
// 升序（自然顺序）
Arrays.sort(array, (a, b) -> a - b);
Arrays.sort(array, Comparator.naturalOrder());  // Java 8+

// 降序
Arrays.sort(array, (a, b) -> b - a);
Arrays.sort(array, Comparator.reverseOrder());  // Java 8+
```

### 2. 字符串排序

```
String[] words = {"banana", "apple", "cherry", "date"};

// 按字典序升序
Arrays.sort(words, (s1, s2) -> s1.compareTo(s2));

// 按字典序降序
Arrays.sort(words, (s1, s2) -> s2.compareTo(s1));

// 按字符串长度排序
Arrays.sort(words, (s1, s2) -> s1.length() - s2.length());

// 先按长度，再按字典序
Arrays.sort(words, (s1, s2) -> {
    int lenDiff = s1.length() - s2.length();
    return lenDiff != 0 ? lenDiff : s1.compareTo(s2);
});
```

### 3. 对象数组排序

```
class Person {
    String name;
    int age;
    
    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    @Override
    public String toString() {
        return name + "(" + age + ")";
    }
}

public class ObjectSortDemo {
    public static void main(String[] args) {
        Person[] people = {
            new Person("Alice", 25),
            new Person("Bob", 20),
            new Person("Charlie", 30),
            new Person("David", 20)
        };
        
        // 按年龄升序
        Arrays.sort(people, (p1, p2) -> p1.age - p2.age);
        System.out.println("按年龄升序: " + Arrays.toString(people));
        
        // 按年龄降序，同名按名字字典序
        Arrays.sort(people, (p1, p2) -> {
            int ageDiff = p2.age - p1.age;  // 年龄降序
            return ageDiff != 0 ? ageDiff : p1.name.compareTo(p2.name);
        });
        System.out.println("年龄降序，名字升序: " + Arrays.toString(people));
    }
}
```

---

## ⚠️ 注意事项

### 1. 整数溢出问题

```
// 错误示例：可能溢出
Arrays.sort(array, (a, b) -> a - b);

// 正确写法：使用比较方法
Arrays.sort(array, (a, b) -> Integer.compare(a, b));
// 或
Arrays.sort(array, Comparator.comparingInt(a -> a));
```

### 2. 空值处理

```
// 处理可能为 null 的元素
Arrays.sort(array, Comparator.nullsFirst(Comparator.naturalOrder()));
Arrays.sort(array, Comparator.nullsLast(Comparator.naturalOrder()));
```

### 3. 多级排序

```
// 链式比较器
Arrays.sort(people, Comparator
    .comparing(Person::getAge)      // 先按年龄
    .thenComparing(Person::getName)  // 再按名字
    .reversed());                    // 降序
```

### 4. 基本类型数组与对象数组的区别

```
// 基本类型数组（int[]） - 只能使用默认排序
int[] primitiveArr = {5, 2, 8};
Arrays.sort(primitiveArr);  // 正确
// Arrays.sort(primitiveArr, comparator);  // 错误！不能使用Comparator

// 对象数组（Integer[]） - 可以使用Comparator自定义排序
Integer[] objectArr = {5, 2, 8};
Arrays.sort(objectArr);  // 正确，默认升序
Arrays.sort(objectArr, Comparator.reverseOrder());  // 正确，使用Comparator
```

---

## 📊 不同写法的等价性

|写法|说明|适用场景|
|---|---|---|
|`o1 - o2`|直接相减|简单整数比较，注意溢出风险|
|`Integer.compare(o1, o2)`|使用包装类方法|安全，推荐使用|
|`Comparator.naturalOrder()`|自然顺序|Java 8+，简洁|
|`(a, b) -> a - b`|Lambda 表达式|Java 8+，简洁|
|匿名内部类|传统方式|Java 8 之前，或需要复杂逻辑|

---

## 💡 最佳实践

1. **避免溢出**: 使用 `Integer.compare()`替代直接相减
    
2. **使用 Lambda**: Java 8+ 推荐使用 Lambda 表达式，代码更简洁
    
3. **链式调用**: 多条件排序使用 `thenComparing()`
    
4. **空值处理**: 如果数组可能包含 null，使用 `nullsFirst()`或 `nullsLast()`
    
5. **性能考虑**: 对于基本类型数组，使用默认排序更快
    
6. **数组类型**: 注意基本类型数组不能使用 Comparator，需要转换为包装类数组
    

---

## 📋 完整示例代码

```
package arraysdemo;

import java.util.Arrays;
import java.util.Comparator;

public class ArraysDemo1 {
    public static void main(String[] args) {
        // toString()方法:将数组转化为字符串
        System.out.println("------toString()方法------");
        int[] arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
        System.out.println(Arrays.toString(arr));

        System.out.println("------binarySearch()方法------");
        // binarySearch()方法:二分查找,数组必须有序
        int[] arr1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
        System.out.println(Arrays.binarySearch(arr1, 5));

        System.out.println("------copyOf()方法------");
        // copyOf()方法:数组拷贝
        int[] arr2 = Arrays.copyOf(arr1, 5);
        System.out.println(Arrays.toString(arr2));

        System.out.println("------copyOfRange()方法------");
        // copyOfRange()方法:数组拷贝(指定范围)
        int[] arr3 = Arrays.copyOfRange(arr1, 2, 5);
        System.out.println(Arrays.toString(arr3));

        System.out.println("------fill()方法------");
        // fill()方法:数组填充
        int[] arr4 = new int[5];
        Arrays.fill(arr4, 10);
        System.out.println(Arrays.toString(arr4));

        System.out.println("------sort()方法------");
        // sort()方法:数组排序,快速排序
        int[] arr5 = {2, 6, 5, 12, 9, 8, 13, 15, 17, 20,
                25, 23, 30, 28, 33, 31, 36, 39, 40, 35};
        Arrays.sort(arr5);
        System.out.println(Arrays.toString(arr5));
        
        System.out.println("------sort() with Comparator------");
        // 使用Comparator进行自定义排序
        Integer[] arr6 = {5, 2, 8, 1, 9, 3};
        Arrays.sort(arr6, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                // 升序：o1 - o2，降序：o2 - o1
                return o1 - o2;  // 升序排序
            }
        });
        System.out.println("自定义升序排序: " + Arrays.toString(arr6));
    }
}
```

---

## 🎯 总结

`Arrays`类是Java集合框架中的一个重要工具类，提供了操作数组的各种静态方法。这些方法大大简化了数组的常见操作，提高了开发效率和代码可读性。特别是通过 `Comparator`接口，我们可以实现高度灵活的自定义排序逻辑。

### 关键要点：

1. 基本数组操作方法：`toString()`, `binarySearch()`, `copyOf()`, `copyOfRange()`, `fill()`
    
2. 排序方法：`sort()`支持默认排序和自定义排序
    
3. 自定义排序：通过 `Comparator`接口实现复杂的排序逻辑
    
4. 现代Java特性：Lambda表达式和方法引用使代码更简洁
    
5. 注意事项：避免整数溢出，正确处理null值，区分基本类型和对象数组
    

熟练掌握这些方法，可以有效提升Java编程效率和代码质量。