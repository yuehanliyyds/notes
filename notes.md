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