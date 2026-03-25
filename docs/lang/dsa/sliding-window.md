滑动窗口算法（sliding window algorithm）:

滑动窗口的核心就是，右指针给窗口扩容，直至抵达扩容限制条件或抵达边界；左指针则是给窗口缩容，以释放限制条件的约束，保证窗口继续向边界移动。

## Longest Substring Without Repeating Characters

```
Given a string s having lowercase characters, find the length of the longest substring without repeating characters. 

Input: s = "geeksforgeeks"
Output: 7 
Explanation: The longest substrings without repeating characters are "eksforg” and "ksforge", with lengths of 7.

Input: s = "aaa"
Output: 1
Explanation: The longest substring without repeating characters is "a"

Input: s = "abcdefabcbb"
Output: 6
Explanation: The longest substring without repeating characters is "abcdef".
```

tip1:

使用双指针模拟滑动窗口的左右边界，right指针扩容（窗口），遇到每个字符都往hashmap存储（key字符，value对应索引，重复字符的索引会被覆盖），当遇到重复字符的时候，将当前索引+1赋值给left指针缩容（窗口）。每次遍历记录窗口长度的最大值（res = right - left + 1）。遍历结束后，即可得出符合窗口规则的最大长度。

[Go代码示例](https://github.com/ashe-c0de/lang-lab/blob/main/golang/algorithm/slide-window.go)