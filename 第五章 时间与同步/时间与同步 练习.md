# 练习：重新发明 NTP

Exercise: Reinventing NTP

**背景**：你的电脑 A 和一台服务器 B 通过网络相连。A 的时钟和 B 的时钟各自独立运行，两者的"现在"存在一个未知的偏移 θ（A 的时间减去 B 的时间）。网络传输有一个未知的延迟 d，且往返延迟可能不对称——A→B 的延迟和 B→A 的延迟不一定相等。你的任务是设计一套消息交换协议，让 A 估算出 θ，从而校正自己的时钟。

**Background**: Your computer A and a server B are connected by a network. A's clock and B's clock run independently, with an unknown offset θ (A's time minus B's time). The network has an unknown delay d, and the round-trip may be asymmetric — the delay from A→B and B→A are not necessarily equal. Your task: design a message exchange protocol so A can estimate θ and correct its own clock.

协议允许 A 向 B 发送消息，B 收到后回复。每条消息可以携带发送时刻和接收时刻的时间戳。你有完全的自由设计消息的条数、顺序和携带的信息。

The protocol allows A to send messages to B, and B to reply. Each message may carry timestamps of send and receive times. You have complete freedom to design the number, order, and content of messages.

---

**问题 1 — 天真方案**

**Question 1 — The naive approach**

A 向 B 发一条消息："现在几点了？" B 收到后立即回复自己的当前时间 T_B。A 收到回复后，将自己的时钟设置为 T_B。这个方案错在哪里？用 θ 和 d 表达 A 校正后的剩余误差。

A sends B a message: "What time is it?" B immediately replies with its current time T_B. Upon receipt, A sets its clock to T_B. What's wrong with this scheme? Express the residual error after correction in terms of θ and d.

*提示：考虑消息在路上的耗时。Hint: consider the time the message spends in transit.*

---

**问题 2 — 往返测量**

**Question 2 — Round-trip measurement**

A 在时刻 t1（按 A 的时钟）发送请求。B 在时刻 t2（按 B 的时钟）收到请求，在时刻 t3（按 B 的时钟）发送回复，并在回复中附上 t2 和 t3。A 在时刻 t4（按 A 的时钟）收到回复。

A sends a request at time t₁ (by A's clock). B receives it at time t₂ (by B's clock), sends a reply at time t₃ (by B's clock), and includes t₂ and t₃ in the reply. A receives the reply at time t₄ (by A's clock).

(a) 用 t1, t2, t3, t4 表达 A→B 的延迟 d_AB 和 B→A 的延迟 d_BA。你能单独求出 d_AB 或 d_BA 吗？你能求出往返总延迟 d_total = d_AB + d_BA 吗？

(a) Express A→B delay d_AB and B→A delay d_BA in terms of t₁, t₂, t₃, t₄. Can you solve for d_AB or d_BA individually? Can you solve for the total round-trip delay d_total = d_AB + d_BA?

*提示：A 只知道 t1 和 t4（自己量的），B 只知道 t2 和 t3（自己量的）。但 A 在收到回复后知道了全部四个时间戳。Hint: A only knows t₁ and t₄ (measured locally); B only knows t₂ and t₃ (measured locally). But after receiving the reply, A knows all four timestamps.*

(b) 用 t1, t2, t3, t4 表达时钟偏移 θ = t_A − t_B（即 A 的时间减去 B 的时间）。假设 d_AB = d_BA，你能否求出 θ 的精确值？

(b) Express the clock offset θ = t_A − t_B (A's time minus B's time) in terms of t₁, t₂, t₃, t₄. Assuming d_AB = d_BA, can you derive the exact value of θ?

*提示：写出 t2 − t1 = d_AB + θ 和 t4 − t3 = d_BA − θ。Hint: write t₂ − t₁ = d_AB + θ, and t₄ − t₃ = d_BA − θ.*

---

**问题 3 — 不对称性的代价**

**Question 3 — The cost of asymmetry**

如果 d_AB ≠ d_BA（比如 A→B 走光纤、B→A 走卫星），你在问题 2(b) 中估算的 θ 的误差是多少？用 d_AB 和 d_BA 的差表达。

If d_AB ≠ d_BA (e.g., A→B goes through fiber, B→A through satellite), what is the error in the θ you estimated in 2(b)? Express it in terms of the difference between d_AB and d_BA.

NTP 的实际做法是：承认不对称性不可消除，但通过选择网络路径相近的服务器来尽量让 d_AB ≈ d_BA。这是否是工程上合理的取舍？你能否设计一个不使用"对称假设"的替代方案？如果不能，为什么不能？

NTP's practical approach: accept that asymmetry cannot be eliminated, but choose servers with similar network paths to approximate d_AB ≈ d_BA. Is this a reasonable engineering tradeoff? Can you design an alternative that doesn't rely on the symmetry assumption? If not, why not?

*提示：考虑你是否能从四个时间戳中提取出除了 d_AB + d_BA 之外的任何关于单程延迟的信息。Hint: consider whether you can extract any information about one-way delays beyond d_AB + d_BA from the four timestamps.*

---

**问题 4 — 从单次测量到稳定同步**

**Question 4 — From a single measurement to stable synchronization**

单次 NTP 测量的 θ 包含网络抖动的噪声。实际 NTP 客户端会连续进行多次测量（比如每 64 秒一次，共 8 次），然后从中筛选出"最好"的一次（通常选择往返延迟最小的一次）来计算 θ。

A single NTP measurement's θ includes noise from network jitter. Real NTP clients make multiple consecutive measurements (e.g., 8 measurements, one every 64 seconds), then select the "best" one (typically the one with the smallest round-trip delay) to compute θ.

为什么选择"往返延迟最小"的测量，而不是"θ 最接近当前时钟"的测量？一旦计算出 θ，为什么 NTP 不是一次性将时钟跳变到校正后的时间，而是缓慢地调整（slew）——每秒只校正几毫秒？

Why pick the measurement with "smallest round-trip delay" rather than the one with "θ closest to current clock"? Once θ is calculated, why does NTP slowly adjust (slew) the clock — correcting only a few milliseconds per second — rather than jumping to the corrected time all at once?

*提示：思考什么样的测量值最可信，以及时钟的突然跳变对运行中的程序（日志、超时、定时器）意味着什么。Hint: think about which measurements are most trustworthy, and what a sudden clock jump means for running programs (logs, timeouts, timers).*

---

**问题 5 — 实现与验证**

**Question 5 — Implement and verify**

用 Python（或你熟悉的语言）实现一个简化的 NTP 客户端模拟。不需要真实的网络——在代码中模拟：
- 一个服务器时钟，以固定的速率单调前进
- 一个客户端时钟，与服务器有一个初始偏移 θ，且以略快（或略慢）的速率漂移
- 网络延迟：每次请求-回复的 d 在一个均值附近随机波动，偶尔出现一个异常大的延迟
- 客户端每 1 秒发起一次 NTP 查询，共运行 60 秒

Implement a simplified NTP client simulation in Python (or your preferred language). No real network needed — simulate: a server clock progressing monotonically at a fixed rate; a client clock with initial offset θ from the server, drifting slightly faster (or slower); network delay: each request-reply's d randomly fluctuates around a mean, with occasional abnormally large spikes; the client initiates an NTP query every 1 second, running for 60 seconds total.

绘制两条曲线：(1) 原始偏移 θ 随时间的变化（未校正时应该线性漂移），(2) 应用 NTP 校正后的偏移随时间的变化。你的实现是否将偏移控制在了一个有限的范围内？

Plot two curves: (1) raw offset θ over time (should drift linearly without correction), (2) NTP-corrected offset over time. Does your implementation keep the offset within a bounded range?

*提示：用问题 2(b) 的公式估算 θ，用问题 4 的策略（选最小 RTT + slow slew）。不要一次性跳变。Hint: use the formula from 2(b) to estimate θ, apply the strategy from question 4 (pick min-RTT measurement + slow slew). Don't jump the clock.*
