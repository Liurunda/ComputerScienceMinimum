# 练习 1 — Splay 树：用"自调整"利用时间局部性

Exercise 1 — Splay Trees: Exploiting Temporal Locality Through Self-Adjustment

Splay 树是 Sleator 和 Tarjan 在 1985 年提出的一种自调整二叉搜索树。每次查找、插入、删除后，被操作的节点通过一系列旋转（称为"splay"操作）被移到根的位置。

A splay tree is a self-adjusting binary search tree proposed by Sleator and Tarjan in 1985. After each search, insertion, or deletion, the operated node is moved to the root through a series of rotations (called a "splay" operation).

---

**(a) 手算**

从一个包含键 {1, 2, 3, 4, 5, 6, 7} 的平衡 BST 开始（根=4）。依次查找 7, 7, 7, 7。每次查找后画出树的结构。观察到什么？

**(a) By hand**

Start with a balanced BST containing keys {1, 2, 3, 4, 5, 6, 7} (root=4). Search for 7, then 7, then 7, then 7. Draw the tree after each search. What do you observe?

*提示：第一次查找 7 时，7 被 splay 到根。第二次查找 7 时，7 已经在根——查找成本从 O(n) 降到 O(1)。如果某个键被反复访问，它在树中的深度会趋近于 1。Hint: After the first search for 7, 7 is splayed to the root. On the second search, 7 is already at the root — lookup cost drops from O(n) to O(1). If a key is accessed repeatedly, its depth converges to 1.*

---

**(b) 实现与测量**

用 Python 实现 Splay 树和普通 BST（不平衡）。生成一个长度为 10^5 的键序列，其中 80% 的访问集中在 10% 的键上（Zipf 分布，模拟真实程序的时间局部性）。分别测量两个树完成全部 10^5 次查找所需的总比较次数。哪个树的累积成本更低？为什么？

**(b) Implement and measure**

Implement a splay tree and an ordinary BST (unbalanced) in Python. Generate a key sequence of length 10^5 where 80% of accesses concentrate on 10% of keys (Zipf distribution, simulating temporal locality in real programs). Measure the total number of comparisons for all 10^5 lookups on each tree. Which tree has lower cumulative cost? Why?

*提示：Splay 树的"自调整"自动将热点数据推到树的上层。这等于在纯软件中实现了一个小型的"缓存"——热点键接近根（类似 L1），冷键被推到深处（类似 DRAM）。这和本章的层级表是否有结构上的对应？Hint: The splay tree's self-adjustment automatically pushes hot data toward the upper levels. This is equivalent to implementing a small "cache" in pure software — hot keys near root (like L1), cold keys pushed deep (like DRAM). Is there a structural correspondence with the hierarchy table in this chapter?*
