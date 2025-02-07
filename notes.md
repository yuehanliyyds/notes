Get Maximum Stability：</p>
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


