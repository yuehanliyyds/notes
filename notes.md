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