
# 代码随想录Day5

leetcode 242, 349, 202, 1

## [leetcode 242](https://leetcode.com/problems/valid-anagram/description/)

这个题之前做过很多次，思路很明确就是用哈希表，统计每个字母出现的次数，然后看是不是相等
这里要记住counter可以直接计算字符串

先写counter的写法，写法上是最简单的

```Python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        from collections import Counter
        s_count = Counter(s)
        t_count = Counter(t)
        return s_count == t_count

```

然后再是一般的常规写法, 这个地方要注意转ascii码用 ord()，以及这里的i是迭代的字符
这里也可以对最后的结果数组先加再减，看看是不是全是0

```Python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        
        s_count = [0]*26
        t_count = [0]*26

        for i in s:
            s_count[ord(i) - ord('a')] += 1
        for i in t:
            t_count[ord(i) - ord('a')] += 1

        return s_count == t_count
```



## [leetcode 349](https://leetcode.com/problems/intersection-of-two-arrays/description/)

第一次直接用ac了，之后还是可以注意一下dict和counter的区别

```Python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        from collections import Counter
        c_1 = Counter(nums1)
        c_2 = Counter(nums2)
        res = []
        for i in c_1:
            if i in c_2:
                res.append(i)
        return res
```

同时这里面也可以直接用set然后求交集，这种python本身的东西要会

```Python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return list(set(nums1)&set(nums2))

```

## [leetcode 202](https://leetcode.com/problems/happy-number/description/)

这个地方陷入的误区，要注意的是这个数字是不是出现过一次，而不是把这个数当成字符串
其次要会用divmod()求商和余数，其他的按照思路写就可以

```Python
class Solution:
    def isHappy(self, n: int) -> bool:
        def get_sum(n):
            temp = 0 
            while n:
                n, r = divmod(n,10)
                temp += r**2
            return temp

        record =set()

        while True:
            n = get_sum(n)
            if n == 1:
                return True

            if n in record:
                return False
            else:
                record.add(n)
        
```
## [leetcode 1](https://leetcode.com/problems/happy-number/description/)

这个地方思路还是比较好理解的，但是对于我来说要注意两个地方
1. enumerate 要会用，同时返回数值和下标，这个一直没记住
2. 注意dict结构如何定义，然后要存dict[index] = value
3. 这个地方key用数字，value用index （这点很好理解，只做记录）

另外，用set做也可以，但是要用nums.index()这个要记住了
   
```Python 
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        record = dict()

        for index, elem in enumerate(nums):
            if target - elem in record:
                return [record[target-elem],index]
            else:
                record[elem] = index

        return []
```            


