# 练习 2 — B 树：用"块匹配"利用空间局部性

Exercise 2 — B-trees: Exploiting Spatial Locality Through Block Matching

B 树（Bayer & McCreight, 1972）是数据库和文件系统中最常用的索引结构。每个节点存储有序的键值数组和指向子节点的指针数组，节点的键数量由分支因子 B 控制。

The B-tree (Bayer & McCreight, 1972) is the most widely used index structure in databases and filesystems. Each node stores a sorted array of keys and an array of child pointers. The number of keys per node is controlled by the branching factor B.

---

**(a) 计算最优 B**

假设你的磁盘页大小为 4 KB，每个键 8 字节，每个指针 8 字节，每个节点需要额外的 16 字节元数据。一个节点包含 k 个键时，占用多少字节？令其不超过 4 KB，k 的最大值（即 B）是多少？

**(a) Compute the optimal B**

Suppose your disk page size is 4 KB. Each key is 8 bytes, each pointer is 8 bytes, and each node requires 16 additional bytes of metadata. How many bytes does a node with k keys occupy? Constraining this to ≤ 4 KB, what is the maximum k (i.e., B)?

*提示：k 个键 = 8k 字节，k+1 个指针 = 8(k+1) 字节，加上 16 字节元数据。求解 8k + 8(k+1) + 16 ≤ 4096。Hint: k keys = 8k bytes, k+1 pointers = 8(k+1) bytes, plus 16 bytes metadata. Solve 8k + 8(k+1) + 16 ≤ 4096.*

---

**(b) 层级穿越的次数**

假设存储了 10^9 个键。普通 BST（B=2）的一次查找平均穿过多少层？用 (a) 中算出的 B 值的 B 树，一次查找穿过多少层？两种树各需要多少次**页面加载**（假设每层需要一次磁盘 IO）？

**(b) Number of hierarchy crossings**

Suppose 10^9 keys are stored. On average, how many levels does a lookup traverse in an ordinary BST (B=2)? In a B-tree with the B value from (a)? How many **page loads** does each tree require (assuming one disk IO per level)?

*提示：BST 的高度 ≈ log₂(10^9) ≈ 30。B 树的高度 ≈ log_B(10^9)。如果你的 B ≈ 250，log₂₅₀(10^9) ≈ ? 这解释了为什么 B 树是磁盘上的默认索引结构。Hint: BST height ≈ log₂(10^9) ≈ 30. B-tree height ≈ log_B(10^9). If your B ≈ 250, log₂₅₀(10^9) ≈ ? This explains why B-trees are the default on-disk index structure.*

---

**(c) B 树为什么利用了局部性**

B 树的一个节点在物理上是一个连续的内存块（或一个磁盘页）。解释这如何同时利用了**空间局部性**（所有键在一个块内连续存放）和**时间局部性**（整块一起被加载到缓存/内存中）。如果 B 树的节点大小恰好等于 CPU 的 L2 缓存行大小（64 字节），会发生什么？

**(c) Why B-trees exploit locality**

A B-tree node is physically a contiguous block of memory (or a disk page). Explain how this simultaneously exploits **spatial locality** (all keys within a block are stored contiguously) and **temporal locality** (the entire block is loaded into cache/memory together). What happens if a B-tree node is sized exactly to the CPU's L2 cache line size (64 bytes)?
