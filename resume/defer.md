← [Ashe wiki](../README.md)

---
defer的特性

- defer的statement总是在其他statement执行之后执行
- 存在多个defer，遵循后进先出LIFO顺序
- defer的statement即使在发生panic的情况下仍然会执行
