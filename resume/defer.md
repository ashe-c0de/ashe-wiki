← [Ashe wiki](../README.md)

---
defer的特性

- defer的statement总是在其他statement执行之后执行
- 存在多个defer，遵循后进先出LIFO顺序
- defer的statement即使在发生panic的情况下仍然会执行

defer和return的执行顺序

return 语句先计算并确定返回值（或赋值给具名返回变量），然后执行 defer，最后函数真正返回。

如果函数返回值是匿名的，defer 无法修改最终返回的结果，因为函数返回的是数值。
如果函数返回值是具名的，defer 可以修改它，因为具名返回值本质上是一个函数作用域内的变量。
