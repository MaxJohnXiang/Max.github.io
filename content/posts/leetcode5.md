---
title: "Longest Palindromic Substring"
date: 2019-11-07T13:42:11+08:00
draft: false
---


>Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

>Example 1:


>Input: "babad"
>Output: "bab"
>Note: "aba" is also a valid answer.


>Example 2:

>Input: "cbbd"
>Output: "bb"


> 首先暴力解法， 找出所有的子串, 然后判断是否是回文，如果是求长度。 

## 暴力解法
```go
	maxLen := 0
	for i := 0; i < n; i++ {
		for j := i; j >= 0; j-- {
			tem := isPalindromic(s[i:j])
			if len(tem) >= maxLen {
				maxLen = len(tem)
			}
		}
	}
```

但是暴力解法重复做了很多次判断，例如字符串cdadbdc
* **aba**
* d**aba**d
* cd**aba**dc

每次都遍历了这几个重复的子串，**很多运算是重复的。**

这个时候我们可以用一个状态state(start,end),来表是这个子串是否是回文，记录下子字符串是否是回文，来避免重复运算

start 是字符串开始的长度

end 是字符串结束的长度

## 拆解子问题 
> if start=end 即 a  is palindromic// 一个字符的时候一定是回文

> if start +1 = end aa if s[startIndex] == s[endIndex]
> 两个字符的时候， 如果第一个字符等于最后一字符，也一定是回文


> if start +2 = end  aba  s[startIndex] == s[endIndex] && 中间那个b是回文 如果开始和最后的字符相等， 只要保证中间的字符是回文

> if start +3 = end  cbbc  s[startIndex] == s[endIndex] &&中间那个bb是回文中间那个b是回文 如果开始和最后的字符相等， 只要保证中间的字符是回文

通过这个分析，我们得到了，如果这个字符串， 开始和结束相等 s[start]
= s[end], 只要中间的子字符串是回文即可保证是回文

所以这个字符串是否是回文可以拆解为这几个字问题。

## 状态转移方程
```
state transition state（s,e）为true 取决于
if 长度为1 无需判断
if 长度为2 s[start] = s[end] 
if 长度为3 s[start] = [end] and state(i+1, j-1)=true
```

### 解题思路


## 找出所有的子串

```go
	for i := 0; i < n; i++ {
		for j := i; j >= 0; j-- {
```

这个时候需要使用自顶向下的遍历方式， 因为state(s,e) 需要依赖于 state(s+1,
e-1),这个状态，
我们需要在计算最长的字符之前，先把state(s+1,e-1)，这些子状态求出来。
例如判断dp[0][3]的时候需要依赖于dp[1][2]
这个状态，所以我们需要先求出后面的结果。再求出前面的。


```
i , j 0 0 true
i , j 0 1 true  //依赖于 [1,0]
i , j 0 2 true  // 依赖[1,1]
i , j 0 3 false //依赖于[1,2]
i , j 1 1 true  // 依赖于[2, 0]
i , j 1 2 true  // 依赖于[2,1]
i , j 1 3 true  // 依赖于[0, 2]
i , j 2 2 true  // 依赖于[1,1]
i , j 2 3 true  // 依赖于[3, 2]
i , j 3 3 true  //依赖于[2,2]
```

```go
func longestPalindrome(s string) string {
	//Corner case
	if len(s) <= 1 { // 肯定是回文，直接返回
		return s
	}
	n := len(s)
	//dp[s][e] true , if s[s:e] is palindromic
	dp := make([][]bool, len(s))
	for i := 0; i < n; i++ {
		dp[i] = make([]bool, len(s))
	}

	maxLen := 0
	res := ""
	for i := 0; i < n; i++ {
		for j := i; j >= 0; j-- {
			if s[i] == s[j] && (j-i <= 2 || dp[i+1][j-1]) {
				dp[i][j] = true
			}
			if dp[i][j] && j-i+1 >= maxLen {
				maxLen = j - i + 1
				res = s[i : j+1]
			}
		}
	}
	return res
}
```

