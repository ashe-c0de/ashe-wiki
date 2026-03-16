中心扩展算法（expand around center algorithm）:

The algorithm expands around each potential center (character or gap) to find the longest palindromic substring.

## Longest Palindromic Substring

```
Given a string s, find the longest substring which is a palindrome. If there are multiple answers, then find the first appearing substring.


Input: s = "forgeeksskeegfor"
Output: "geeksskeeg"
Explanation: The longest substring that reads the same forward and backward is "geeksskeeg". Other palindromes like "kssk" or "eeksskee" are shorter.

Input: s = "Geeks"
Output: "ee"
Explanation: The substring "ee" is the longest palindromic part in "Geeks". All others are shorter single characters.

Input: s = "abc"
Output: "a"
Explanation: No multi-letter palindromes exist. So the first character "a" is returned as the longest palindromic substring.
```

思路：遍历整个字符串，在任意index处取[0, index]的（回文）中心，以左右指针从中心向两侧扩展，并以此校验是否符合回文规则

tip1:

index值为奇数或偶数，需要两个case来取（回文）中心，进而决定左右指针的索引位置，这里可以通过对2取余的方式统一成一个case来简化代码

```go
var i int
left := i / 2
right := left + i%2
// 当i=4 left=2, right=2（偶数，左右指针重合）; i=5, left=2 right=3（奇数，左右指针相邻）
```

tip2:

字符串长度（假设n）遍历只能保证奇数长度回文被覆盖，但偶数长度回文的中心可能在字符间隙中，如果不遍历 2n-1，就可能漏掉最长回文，比如abb最长回文是bb，但是以n=3去取回文中心时，中心点落在了index=1处，[left] != [right]，n>3已经结束遍历，那么就无法得出正确结果。

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/algorithm/char-gap.png)

为了避免这种场景，我们需要保证回文中心始终落在字符间隙，而不是落在字符，因此把字符间隙视（抽象）作一个单位
那么aba奇数长度拥有偶数个间隙，abba偶数长度拥有奇数个间隙
n长度的字符串总共单位即2*n-1

[Go代码示例](https://github.com/ashe-c0de/lang-lab/blob/main/golang/algorithm/expand-around-center.go)