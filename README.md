# Leetcode_Python_150
Challenge Top interview 150 Leetcode 

Arrays/Strings
1. Merge Array
    You are given two integer arrays nums1 and nums2, sorted in non-decreasing order,
    and two integers m and n, representing the number of elements in nums1 and nums2 respectively.
    Merge nums1 and nums2 into a single array sorted in non-decreasing order.
    The final sorted array should not be returned by the function, but instead be stored inside the array nums1.
    To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged,
    and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

```python

class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        new_array = nums1[0:m]
        nums1.clear()
        nums1.extend(new_array)
        nums1.extend(nums2)
        nums1.sort()
```

or
    
```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        for i in range(n):
            nums1[m+i] = nums2[i]
        nums1.sort()

```

2. Remove Element
    Given an integer array nums and an integer val, remove all occurrences of val in nums in-place.
    The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.
    Consider the number of elements in nums which are not equal to val be k, to get accepted, you need to do the following things:
    Change the array nums such that the first k elements of nums contain the elements which are not equal to val.
    The remaining elements of nums are not important as well as the size of nums.
    Return k.
   
```python

    class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        
        i = 0 
        for j in range(len(nums)):
            if nums[j] != val:
                nums[i] = nums[j]
                i += 1
        return i

```

3. Remove Duplicates from Sorted Array
   Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only       once. The relative order of the elements should be kept the same. Then return the number of unique elements in nums.

   Consider the number of unique elements of nums to be k, to get accepted, you need to do the following things:

   Change the array nums such that the first k elements of nums contain the unique elements in the order they were present in nums             initially. The remaining elements of nums are not important as well as the size of nums.
   Return k.

```python

class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        
        if not nums:
            return 0 
        
        i = 0
        for j in range(len(nums)):
            if nums[j] != nums[i]:
                i += 1
                nums[i] = nums[j]
        
        return i + 1
        
```

9. Jump Game (medium)
    You are given an integer array nums. You are initially positioned at the array's first index,
    and each element in the array represents your maximum jump length at that position.
    Return true if you can reach the last index, or false otherwise.

```python

    class Solution:
    def canJump(self, nums: List[int]) -> bool:
        jump = 0

        for n in nums: 
            if jump < 0:
                return False
            elif n > jump:
                jump = n
            jump -= 1
        return True

```

10. Jump Game II (medium)
    You are given a 0-indexed array of integers nums of length n. You are initially positioned at nums[0].
    Each element nums[i] represents the maximum length of a forward jump from index i. In other words,
    if you are at nums[i], you can jump to any nums[i + j] where:

    0 <= j <= nums[i] and
    i + j < n
    Return the minimum number of jumps to reach nums[n - 1]. The test cases are generated such that you can reach nums[n - 1].

```python

    class Solution:
    def jump(self, nums: List[int]) -> int:
        n = len(nums)
        max_reach = nums[0]
        end = nums[0]
        jump = 1

        if n == 1:
            return 0

        for i in range(1, n - 1):
            max_reach = max(max_reach, i + nums[i])
            if i == end:
                end = max_reach
                jump += 1
        
        return jump

```

11. H-Index
    Given an array of integers citations where citations[i] is the number of citations a researcher received for their ith paper, return        the researcher's h-index.

    According to the definition of h-index on Wikipedia: The h-index is defined as the maximum value of h such that the given researcher        has published at least h papers that have each been cited at least h times.


```python

    class Solution:
    def hIndex(self, citations: List[int]) -> int:
        
        n = len(citations)
        citations.sort()

        for index, val in enumerate(citations):
            if n - index <= val:
                return n - index

        return 0
```

12 . Insert Delete GetRandom O(1)
    RandomizedSet() Initializes the RandomizedSet object.
    bool insert(int val) Inserts an item val into the set if not present. Returns true if the item was not present, false otherwise.
    bool remove(int val) Removes an item val from the set if present. Returns true if the item was present, false otherwise.
    int getRandom() Returns a random element from the current set of elements (it's guaranteed that at least one element exists when this       method is called). Each element must have the same probability of being returned.
    You must implement the functions of the class such that each function works in average O(1) time complexity.

```python

    class RandomizedSet:

    def __init__(self):
        self.lst = []
        self.idx_map = {}

    def search(self, val):
        return val in self.idx_map

    def insert(self, val):
        if self.search(val):
            return False

        self.lst.append(val)
        self.idx_map[val] = len(self.lst) - 1
        return True

    def remove(self, val):
        if not self.search(val):
            return False

        idx = self.idx_map[val]
        self.lst[idx] = self.lst[-1]
        self.idx_map[self.lst[-1]] = idx
        self.lst.pop()
        del self.idx_map[val]
        return True

    def getRandom(self):
        return random.choice(self.lst)

        
```
13. Product of Array Except Self
    Given an integer array nums, return an array answer such that answer[i] is equal to the product of all the elements of nums except             nums[i].
    The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.
    You must write an algorithm that runs in O(n) time and without using the division operation.

```python

    class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        
        n = len(nums)
        answer = [1] * n  # Initialize the result array with 1s

        # Compute prefix products
        prefix = 1
        for i in range(n):
            answer[i] = prefix  # Store the prefix product
            prefix *= nums[i]  # Update prefix product for the next element

        # Compute suffix products and multiply with prefix products
        suffix = 1
        for i in range(n - 1, -1, -1):  # Traverse from right to left
           answer[i] *= suffix  # Multiply suffix product with the stored prefix product
           suffix *= nums[i]  # Update suffix product for the next element

        return answer
```

14. Gas Station
   There are n gas stations along a circular route, where the amount of gas at the ith station is gas[i].

    You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from the ith station to its next (i + 1)th station. You begin the journey with     an empty tank at one of the gas stations.

    Given two integer arrays gas and cost, return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise     return -1. If there exists a solution, it is guaranteed to be unique.

```python

    class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        if sum(gas) < sum(cost):
            return -1
        
        current_gas = 0
        start = 0

        for i in range(len(gas)):
            current_gas += gas[i] - cost[i]

            if current_gas < 0:
                current_gas = 0
                start = i + 1
        
        return start
```

15. Candy
   There are n children standing in a line. Each child is assigned a rating value given in the integer array ratings.
    You are giving candies to these children subjected to the following requirements:
    Each child must have at least one candy.
    Children with a higher rating get more candies than their neighbors.
    Return the minimum number of candies you need to have to distribute the candies to the children.

```python
    class Solution:
    def candy(self, ratings: List[int]) -> int:
        n = len(ratings)
        total_candies = n
        i = 1

        while i < n:
            if ratings[i] == ratings[i - 1]:
                i += 1
                continue

            current_peak = 0
            while i < n and ratings[i] > ratings[i - 1]:
                current_peak += 1
                total_candies += current_peak
                i += 1
            
            if i == n:
                return total_candies

            current_valley = 0
            while i < n and ratings[i] < ratings[i - 1]:
                current_valley += 1
                total_candies += current_valley
                i += 1

            total_candies -= min(current_peak, current_valley)

        return total_candies
    
```

but i like this one

```python

    class Solution:
    def candy(self, ratings: List[int]) -> int:
        n = len(ratings)
        candies = [1] * n 

        for i in range(1, n):
            if ratings[i] > ratings[i-1]:
                candies[i] = candies[i-1] + 1

        for i in range(n-2, -1, -1):
            if ratings[i] > ratings[i+1]:
                candies[i] = max(candies[i], candies[i+1] + 1)
        
        return sum(candies)

```
   
