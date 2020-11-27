---
title:  "Arts Week 1"
date:   2019-07-11
categories: ["arts"]
---

## Review:

[how browsers work][how-browser-work]

普通的tokenizer, parser对HTML不管用，因为html容错率非常高

# 浏览器工作原理：
```
tokenizer -> parser -> 生成DOM Tree
CSS被parser生成CSSStyleSheet Tree. (Firefox blocks all scripts when
there is a style sheet that is still being loaded and parsed)

两者再合体生成render tree，这个地方很难理解，但是可以确定的是display:none的Node
以及无需显示的Node都不出现在render tree.
layout是把render tree的nodes搞出position, dimension.
然后Paint出来全部。
```

## Tips

1. In Chrome debug tool: design mode controls whether the entire document is editable.
```
In debug console:
document.designMode = "on"
```
enable之后可以直接在web page上修改页面

2. In Chrome debug tool: in network, for API request, we can copy request as cURL.
```
Run it in terminal, can be used by script.
更深入研究发现 postman更适合直接发api request, 无需cookie, 只需要auth code
cURL request 如果直接Copy过来含有很多杂质 比如cookie。去除这些之后 等同于postman
```

3. 另外花时间研究了webapp2 生成cookie的方法 cookie in webapp2其实是Usersession 每次访问都生成session存入db. data是加密的。session还可以作为dict加附加值


4. Cross-site_request_forgery
是一种挟制用户在当前已登录的Web应用程序上执行非本意的操作的攻击方法


## Share

[How to rock a design interview][How-to-rock-design-interview]

Systems Design Topics

* `Concurrency`. Do you understand threads, deadlock, and starvation? Do you know how to parallelize algorithms? Do you understand consistency and coherence?

* `Networking`. Do you roughly understand IPC and TCP/IP? Do you know the difference between throughput and latency, and when each is the relevant factor?

* `Abstraction`. You should understand the systems you’re building upon. Do you know roughly how an OS, file system, and database work? Do you know about the various levels of caching in a modern OS?

* `Real-World Performance`. You should be familiar with the speed of everything your computer can do, including the relative performance of RAM, disk, SSD and your network.

* `Estimation`. Estimation, especially in the form of a back-of-the-envelope calculation, is important because it helps you narrow down the list of possible solutions to only the ones that are feasible. Then you have only a few prototypes or micro-benchmarks to write.

* `Availability and Reliability`. Are you thinking about how things can fail, especially in a distributed environment? Do know how to design a system to cope with network failures? Do you understand durability?

### [Scalable Web Architecture and Distributed Systems][scale-web-design]
阅读的过程中，很多知识点似曾相识。
partition->shards-> we could use consistent hashing to find shards

储存的时候可以依照地理位置分replication.

文中的设计例题是 设计一个图片储存服务 关注的2点：
1. scalability storage
2. fast access of data
实现1需要partition
实现2比较有趣，可以使用caches, proxies, indexes, load balancer
![Web Architecture Graph](/img/web_designn_arch.png){:class="img-responsive"}

### Algorithm
https://leetcode.com/explore/learn/card/recursion-i/250/principle-of-recursion/1440/
Write a function that reverses a string. The input string is given as an array of characters char[].

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

```
def reverseString(self, s):
  """
  :type s: List[str]
  :rtype: None Do not return anything, modify s in-place instead.
  """
  def helper(start, end, ls):
    if start >= end:
        return

    # swap the first and last element
    ls[start], ls[end] = ls[end], ls[start]        

    return helper(start+1, end-1, ls)

  helper(0, len(s)-1, s)
```

[how-browser-work]: https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/

[How-to-rock-design-interview]: http://www.palantir.com/2011/10/how-to-rock-a-systems-design-interview/

[scale-web-design]: http://www.aosabook.org/en/distsys.html
