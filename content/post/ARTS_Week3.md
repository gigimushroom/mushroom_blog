---
title:  "Arts Week 3"
date:   2019-10-22
categories: ["arts"]
---

## Algorithm
https://leetcode.com/problems/find-pivot-index/
```
def pivotIndex(self, nums):
    total = sum(nums)
    leftsum = 0
    for i, x in enumerate(nums):
        if leftsum == total - x - leftsum:
            return i
        leftsum += x
    return -1
```
python enumerate是一个强大的枚举function

## Review:
监控系统的设计
学习了左耳的文章 监控系统的设计需要两个目的: 
1. 体检
2. 诊断
Latency, count, 95th percentile.
还学习了datadog的使用以前成熟product是如何使用datadog。
解决了工作中的问题，成功设计了metrics, alerts监控远程第三方系统 Lattency, failure count.

## Tips
python JSON dump 会把Object‘s key变成string.

原因是[JSON][json]不支持integer as key.

同样apply to javascript, js里Object key必须是string或者symbol.

## Share

[Circuit Breaker Pattern][circuit-breaker]
当远程服务没有响应，大量的访问会造成本机的系统资源浪费或者无响应。这个设计模式解决了此类问题：

当远程服务没有响应或者挂机, requests达到failure count之后将不会再访问远程服务。直接state -> open, return error to client.

当reset timeout fires, state变成half open。下一个request访问远程服务。如果成功,回复成功状态。不成功的话重新开始失败计数。

这个是个非常好的golang练习，有机会需要按照文中的ruby代码翻译出。

[json]: https://www.json.org/

[circuit-breaker]: https://martinfowler.com/bliki/CircuitBreaker.html

[scale-web-design]: http://www.aosabook.org/en/distsys.html
