---
title: "Remove Nth Node From End of List"
date: 2019-11-12T23:31:32+08:00
draft: false
tags: ["leetcode", "双指针", "two pointer"]
---

## Description
>Given a linked list, remove the n-th node from the end of list and return its head.
>
>Example:
>
>
>Given linked list: 1->2->3->4->5, and n = 2.
>
>After removing the second node from the end, the linked list becomes 1->2->3->5.
>
>Note:
>Given n will always be valid.


## Thinking
因为链表是没有下标的， 所以找到第n 个， 或者倒数第n个， 或者找中间位置，都必须通过快慢指针实现。

>  1. 找到第n个，快指针先走length - n个位置 , 再快慢指针一起走，等快指针走完了， 慢指针刚好走了n个。
>  2. 找到中间位置， 同时间开始走，快指针速度是满指针两倍， 快指针走完了, 满指针走到中间
>  3. 找到第n个位置， 快指针走n个位置，再快慢指针一起走，
     等快指针走完了，满指正走到倒数第n个


## Solution 

```go
func removeNthFromEnd(head *ListNode, n int) *ListNode {

	start := &ListNode{}

	slow, fast := start, start

	start.Next = head

	//fast move n fast

	//n always valid
        //初始化指针的值
	for i := 1; i <= n+1; i++ {
		fast = fast.Next
	}

        //快慢指针一起走，等快指针结束了，满指针到n的位置
	for fast != nil {
		fast = fast.Next
		slow = slow.Next
	}

        //删除第n个节点
	slow.Next = slow.Next.Next

	return start.Next
}
```
