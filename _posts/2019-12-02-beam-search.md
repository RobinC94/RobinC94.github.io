---
layout: post
title:  "Beam Search"
date:   2019-12-02
excerpt: "Beam Search Algorithm (Draft by Andrew Jungwirth)"
tag:
- algorithm
- graph
- sequence
- search
---

[Original page](http://jhave.org/algorithms/graphs/beamsearch/beamsearch.shtml)

Beam Search: 在 seq2seq 的 inference 时搜索得分最高的序列，基于贪心的思想，同时限制内存的使用大小。

使用广度优先策略建立搜索树，在树的每一层，按照得分对节点进行排序，然后仅留下集束宽度（Beam Width）个数的节点，仅这些节点在下一层继续扩展，其他节点就被剪掉了。

（注意：如果集束宽度无穷大，那该搜索就是宽度优先搜索）

步骤如下：

1.将初始节点插入到list中，

2.将节点出堆，如果该节点是目标节点，则算法结束；

3.否则扩展这些节点并排序，取集束宽度的节点入堆，然后到第二步继续循环。

4.算法结束的条件是找到最优解或者堆为空。
