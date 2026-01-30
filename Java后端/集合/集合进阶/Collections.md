Collections 工具类确实为 Java 集合操作提供了极大的便利。下面这个表格总结了其核心方法，方便你快速查阅。

| 方法类别        | 方法名                                                    | 功能描述                                   | 关键点/注意事项                                                |
| ----------- | ------------------------------------------------------ | -------------------------------------- | ------------------------------------------------------- |
| **排序操作**​   | `sort(List<T> list)`                                   | 对列表按自然升序排序。                            | 列表元素需实现 `Comparable`接口。                                 |
|             | `sort(List<T> list, Comparator<T> c)`                  | 根据自定义比较器对列表排序。                         | 提供灵活的排序逻辑，如降序或按对象属性排序。                                  |
|             | `reverse(List<?> list)`                                | 反转列表中元素的顺序。                            | 直接修改原列表。                                                |
|             | `shuffle(List<?> list)`                                | 随机打乱列表元素顺序。                            | 常用于洗牌、随机化数据。                                            |
| **查找与比较**​  | `binarySearch(List<T> list, T key)`                    | 使用二分查找法在**已排序**列表中查找元素。                | **前提是列表必须已排序**，否则结果不确定。                                 |
|             | `max(Collection<T> coll)`/ `min(...)`                  | 返回集合中的最大或最小值（自然顺序）。                    | 元素需实现 `Comparable`接口。                                   |
|             | `max(Collection<T> coll, Comparator<T> c)`/ `min(...)` | 根据自定义比较器返回最大或最小值。                      | 可用于比较未实现 `Comparable`接口的对象或自定义规则。                       |
|             | `frequency(Collection<T> coll, Object o)`              | 返回指定元素在集合中出现的次数。                       | -                                                       |
| **修改与替换**​  | `swap(List<T> list, int i, int j)`                     | 交换列表中指定索引位置的元素。                        | -                                                       |
|             | `fill(List<T> list, T obj)`                            | 使用指定元素替换列表中的所有元素。                      | 会覆盖原内容，列表大小不变。                                          |
|             | `replaceAll(List<T> list, T oldVal, T newVal)`         | 将列表中所有出现的旧值替换为新值。                      | -                                                       |
|             | `copy(List<T> dest, List<T> src)`                      | 将源列表的所有元素复制到目标列表。                      | **目标列表的长度必须大于等于源列表**，否则抛出 `IndexOutOfBoundsException`。  |
| **不可变集合**​  | `unmodifiableXxx(Collection<T> c)`                     | 创建指定集合的**只读**视图（如 `unmodifiableList`)。 | 任何修改操作都会抛出 `UnsupportedOperationException`。修改原集合会影响此视图。 |
|             | `singletonXxx(T o)`                                    | 创建包含单个元素的不可变集合（如 `singletonList`)。     | 常用于需要传递单个元素集合的场景，避免创建开销。                                |
|             | `emptyXxx()`                                           | 返回一个空的不可变集合（如 `emptyList`)。            | 返回单例，节省内存，适用于需要返回空集合的方法。                                |
| **线程安全包装**​ | `synchronizedXxx(Collection<T> c)`                     | 将指定集合包装成线程安全的版本（如 `synchronizedList`)。 | **注意**：迭代操作仍需手动同步，否则可能产生并发修改异常。                         |
| **其他实用工具**​ | `addAll(Collection<T> c, T... elements)`               | 将可变参数的所有元素添加到指定集合中。                    | 批量添加元素非常方便。                                             |
|             | `rotate(List<T> list, int distance)`                   | 将列表元素循环移动指定距离。                         | 正数右移，负数左移。移出末尾的元素会从开头出现。                                |
|             | `disjoint(Collection<T> c1, Collection<T> c2)`         | 检查两个集合是否**没有交集**，无相同元素则返回 `true`。      | -                                                       |

### 💡 使用建议与最佳实践

- **排序性能**：`Collections.sort()`底层会根据数据规模和列表类型选择高效算法（如 TimSort），对于大数据量且有严格性能要求的场景，可考虑使用 `Arrays.parallelSort()`。
    
- **线程安全**：使用 `synchronizedXxx()`方法包装的集合，在迭代时必须**手动加锁**，否则仍是非线程安全的。
    
- **选择合适集合**：`Collections`工具类很强大，但选择正确的集合类型（如 `ArrayList`, `HashSet`, `HashMap`）是高效操作的基础。
    

希望这个总结能帮助你更好地掌握 Collections 工具类。如果你对某个方法的具体用法或特定场景有疑问，我可以提供更详细的例子。