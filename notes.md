1. Get Maximum Stability：</p>
https://www.fastprep.io/problems/amazon-get-max-stability

```
class Solution:
  def getMaxStability(self, reliability: List[int], availability: List[int]) -> int:
        MODULO = 10**9 + 7
        n = len(reliability)
        
        servers = sorted(zip(availability, reliability))

        max_stability = 0
        reliability_sum = 0
        for i in range(n):
            reliability_sum += reliability[i]
        
        for i in range(n):
            min_avail = servers[i][0]
            if i != 0:
                reliability_sum -= servers[i-1][1]
            max_stability = max(max_stability, (min_avail * reliability_sum) % MODULO)

        return max_stability
```

2. Get Minimum Total Distance：</p>
https://www.fastprep.io/problems/amazon-get-min-total-distance
```
class Solution:
  def getMinTotalDistance(self, dist_centers: List[int]) -> int:
        dist_centers.sort()
        n = len(dist_centers)
        
        median_index = n // 2
        left_median = dist_centers[median_index // 2]
        right_median = dist_centers[(median_index + n) // 2]

        total_distance = 0
        for pos in dist_centers:
            total_distance += min(abs(pos - left_median), abs(pos - right_median))
        
        return total_distance
```
思路：最小距离问题 -> 最优中位数选点</p>
TC：O(nlogn),由于dist_centers.sort()的tc是这个

3. Process Queries on Cart:</p>
https://www.fastprep.io/problems/amazon-process-queries-on-cart
```
class Solution:
    def processQueriesOnCart(self, items: List[int], query: List[int]) -> List[int]:
        # slice操作，相当于 items[0:len(items)]
        cart = items[:]
        
        for q in query:
            if q > 0:
                cart.append(q)
            elif q < 0:
                item_to_remove = abs(q)
                if item_to_remove in cart:
                    # remove(x) 只删除第一个匹配项, 如果x不存在会报错
                    cart.remove(item_to_remove)
        
        return cart
```
上述方法tc过大，无法通过所有的test cases
```
class Solution:
    def processQueriesOnCart(self, items: List[int], query: List[int]) -> List[int]:
        # 用deque存储cart
        cart = items[:]
        # 用字典记录每个元素对应的索引队列
        indices = {}
        
        # 初始化索引队列
        for i, item in enumerate(cart):
            if item not in indices:
                indices[item] = deque()
            indices[item].append(i)

        # 处理查询
        for q in query:
            if q > 0:
                # 添加新元素
                cart.append(q)
                if q not in indices:
                    indices[q] = deque()
                indices[q].append(len(cart) - 1)
            else:
                # 删除元素
                target = abs(q)
                if target in indices and indices[target]:
                    remove_index = indices[target].popleft()
                    cart[remove_index] = None
   
        # 返回未被删除的元素
        return [item for item in cart if item is not None]
```
4. Get Maximum: </p>
https://www.fastprep.io/problems/amazon-get-maximum</p>
拆解 range(j-1, -1, -1)</p>
start = j-1 → 从 j-1 开始（即 j 左边的元素）</p>
stop = -1 → 循环到 0（因为 stop 不包含 -1，所以会到 0）</p>
step = -1 → 每次减少 1，即倒序遍历
```
def getMaximum(numProducts):
    n = len(numProducts)
    max_sum = 0  # 记录最大能取的商品数量
    
    for j in range(n):  # 选择右端点 j
        x_j = numProducts[j]  # 以 numProducts[j] 作为最后一个取的堆
        sum_so_far = x_j  # 当前方案的商品总数
        max_sum = max(max_sum, sum_so_far)  # 更新全局最大值
        
        current_pick = x_j  # 记录当前能取的最大值
        for i in range(j-1, -1, -1):  # 向左扩展
            pick_i = min(numProducts[i], current_pick - 1)  # 取严格递减的数量
            if pick_i <= 0:  # 无法取到有效值，停止扩展
                break # break 语句用于提前终止 for 循环
            
            sum_so_far += pick_i  # 更新当前总数
            current_pick = pick_i  # 更新下一次能取的最大值
            max_sum = max(max_sum, sum_so_far)  # 继续更新最大值
    
    return max_sum  # 返回最终结果
```
5. Get Data Dependence Sum (Fungible)：</p>
https://www.fastprep.io/problems/amazon-get-data-dependence-sum</p>
/（浮点数除法, True Division）</p>
//（整数除法, Floor Division）
```
class Solution:
  def getDataDependenceSum(self, n: int) -> int:
    sum = 0
    k = 1
    while k <= n:
        x = n // k
        k_end = n //x
        sum += x
        k = k_end + 1
    return sum
```
6. Minimum Starting Health to Win the Game：</p>
https://www.fastprep.io/problems/amazon-minimum-starting-health-to-win-the-game
```
class Solution:
  def minimumStartingHealth(self, power: List[int], armor: int) -> int:
    total_damage = sum(power)
    max_power = max(power)
    max_reduction = min(max_power, armor)

    return total_damage - max_reduction + 1
```
思路：Since armor can only be used in one round, we should use it where it reduces the most damage.
We apply it on the largest power[i] round.</p>
tc: O(n)</p>
7. Maximum Score in Balanced String:</p>
https://www.fastprep.io/problems/maximum-score-in-balanced-string
```
class Solution:
  def maximumScoreInBalancedString(self, s: str) -> int:
    sum = 0
    rightstack = deque()
    leftstack = deque()
    for i,s in enumerate(s):
      if s == '(':
        leftstack.append(i)
      elif s == ')':
        rightstack.append(i)
    while leftstack and rightstack:
        rightbrace = rightstack.pop()
        leftbrace = leftstack.popleft()
        sum += (rightbrace - leftbrace) * (rightbrace > leftbrace)
    return sum
```
deque: 双端队列</p>
from collections import deque</p>
尾部追加 append(x):	O(1)</p>	
尾部弹出 pop():	O(1)</p>
头部追加 appendleft(x):	O(1)</p>
头部弹出 popleft():	O(1)</p>
索引访问 dq[i]: O(n)</p>
enumerate(s) 遍历字符串 s，并同时返回：</p>
i 👉 当前字符的索引（位置）</p>
s 👉 当前字符（'(' 或 ')'）</p>
8. Assign Tasks to Servers：</p>
https://www.fastprep.io/problems/amazon-assign-tasks
```
class Solution:
    def assignTasks(self, servers: int, requests: List[int]) -> List[int]:
        heap = [(0, i) for i in range(servers)]   # heap 维护 (负载, 服务器索引)，用于高效找到负载最小的服务器，初始化时负载都是0
        heapq.heapify(heap)  # 建立最小堆
        result = []

        for r in requests:
            temp = []
            
            # 取出所有 `0 ~ r` 范围内的服务器（ 过滤掉超出 r 范围的服务器）
            # heap[0][1] 是最小负载的服务器索引，如果 heap[0][1] > r，说明该服务器 索引超出 r 的限制。
            # heappop(heap) 取出 超出 r 限制的服务器，暂存到 temp，稍后恢复到堆里。
            while heap and heap[0][1] > r:
                temp.append(heapq.heappop(heap))
            
            # 选择最少负载的服务器，heappop删除并返回最小值
            load, server = heapq.heappop(heap)
            result.append(server)

            # 更新负载，heappush插入元素并维护堆结构
            heapq.heappush(heap, (load + 1, server))

            # 恢复其他服务器
            for item in temp:
                heapq.heappush(heap, item)

        return result
```
题目理解：</p>
servers：服务器数量，从 0 到 servers-1 编号。</p>
requests：一个列表，表示每个请求可以被分配到的服务器范围 0 到 requests[i]（包含 requests[i]）</p>
关于堆：</p>
import heapq</p>
Python 自带 heapq 模块，它实现的是 最小堆（Min Heap），可以高效获取 最小元素。</p>
9. Max Likes：</p>
https://www.fastprep.io/problems/tiktok-max-likes
```
from collections import Counter

def maximumLikes(prediction):
    MOD = 10**9 + 7
    
    # 统计每个趋势的总点赞数
    freq = Counter(prediction)
    max_trend = max(freq.keys()) if freq else 0

    # 构建新的数组，index为趋势值，value为该趋势的总和
    trend_likes = [0] * (max_trend + 1)
    for trend, count in freq.items():
        trend_likes[trend] = trend * count

    # 应用打家劫舍逻辑
    dp = [0] * (max_trend + 1)
    dp[0] = trend_likes[0]
    if max_trend >= 1:
        dp[1] = max(trend_likes[0], trend_likes[1])

    for i in range(2, max_trend + 1):
        dp[i] = max(dp[i - 1], dp[i - 2] + trend_likes[i])

    return dp[max_trend] % MOD
```
思路：打家劫舍变形问题，使用dp（（1）确定递推公式；（2）注意边界值；（3）遍历顺序）</p>
不同点：这题真正的限制是不能选择相邻的数值，而不是相邻的天数！</p>
统计prediction中每个数的出现次数，转化为最大不相邻数的选取问题，使用dp</p>
10. Match and Swipe:</p>
Several TikTok creators are participating in a trending challenge called 
There are k creators, labeled from 1 to k, and they start with a shared video sequence represented by the string videoSequence.</p>
How the challenge works: Each creator takes turns (creator 1 goes first, followed by creator 2, and so on, cycling back to creator 1 after creator k's turn). On each turn, a creator can remove any two consecutive matching clips (represented by matching letters in videoSequence) and merge the remaining parts of the sequence back together.</p>

The game continues until there are no consecutive matching clips left.
If a creator cannot make a move, they lose, and the challenge ends.
Given that all creators play strategically, determine the number of the creator who will lose.</p>
Example:

Given: videoSequence = "pzwoowz"， k = 3

Step-by-Step Actions:

Creator 1: Remove the consecutive pair w and w → new sequence: pzwwz</p>
Creator 2: Remove the consecutive pair z and z → new sequence: pzz</p>
Creator 3: Remove the consecutive pair z and z → new sequence: p</p>
Creator 1: Cannot make a valid move → loses.</p>
Answer: 1</p>
Function Description:</p>
Complete the function: findLoser

Parameters:

string videoSequence – the shared video sequence.</p>
int k – the number of creators participating in the challenge.</p>
Returns:

int – the creator who cannot make a move.
```
def findLoser(videoSequence, k):
    stack = []
    current_player = 1

    for char in videoSequence:
        if stack and stack[-1] == char:
            stack.pop()
            current_player = (current_player % k) + 1
        else:
            stack.append(char)

    return current_player
```
思路：能消掉的数对是固定的，用栈模拟，求最后轮到谁</p>
stack[-1]：获取栈顶元素。因为 Python 中索引 -1 表示最后一个元素</p>
TC：O(n)</p>
