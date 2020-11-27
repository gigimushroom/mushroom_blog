---
title:  "Arts Week 4"
date:   2019-10-30
categories: ["arts"]
---

## Algorithm
[Largest Number At Least Twice of Others](https://leetcode.com/problems/largest-number-at-least-twice-of-others)

解法粗暴：第一次循环找到最大 第二次循环找是否满足twice条件

## Review:
阅读了[refactoring Improving the Design of Existing Code][refactoring]第二章。

refactoring的好处是make the code easier to understand, cheaper to modify.

refactoring可以提高软件设计.
有些Small code/feature实现的时候没有对系统有完整理解，重复多余代码造成系统生涩难懂。重构可以删除重复代码.

其他好处：
- clarifying the structure of program
- find bugs faster
- make development faster

一个良好的设计越推敲的时间长 会更耐操作 能走得更远

做代码审核最好和最初作者一起code review

作者观点：
**Codebase health impacts productivity**

Continues Integration benefits:
1. Ensure master is healthy
2. Learn to break large feature into small chunks
3. Use feature toggles to switch off any in-progress features

How to work with legacy code:
1. Add test coverage
2. Then refactor piece by piece, or refactor the place you visit very often

## Tips
- 在写feature得时候 如果把测试先写好 会让feature实现更快
- 多回答同事的问题 把缘由思考清楚 看懂上下关系的代码 不要害怕看open source code 一定要追溯问题的根源
- 跟随最强的人 走在浪尖 做不做最难的活不重要 重要的是和最聪明最有技术的人工作
- 多写文档 多深度思考 
- 能做 会写 能说

## Share
[Branch By Abstraction][Branch-by-ab]

Making a large-scale change to a software system in gradual way that allows you to release the system regularly while the change is still in-progress.

解决了我工作中的问题。可以使用一个抽象的interface，内部根据不同逻辑 使用新或者旧的数据 逐步换成所有case都是用新数据。

[Git Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)


[refactoring]: https://martinfowler.com/books/refactoring.html
[Branch-by-ab]: https://martinfowler.com/bliki/BranchByAbstraction.html
