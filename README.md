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

16. Trapping Rain Water
   Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.
    Example 1:
    Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
    Output: 6
    Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.

    Example 2:
    Input: height = [4,2,0,3,2,5]
    Output: 9
    Constraints:
    n == height.length
    1 <= n <= 2 * 104
    0 <= height[i] <= 105

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        if not height or len(height) < 3:
            return 0
        
        left = 0 # Left pointer
        left_max = 0 # Left max bar
        right = len(height) - 1 # Right pointer
        right_max = 0 # Right max bar
        total_water = 0 # Accumulate trapped water

        while left < right:
            if height[left] < height[right]:
                # Process the left pointer
                if height[left] >= left_max:
                    left_max = height[left]
                else:
                    total_water += left_max - height[left]
                
                left += 1 # move left pointer
            else:
                # Process the right pointer
                if height[right] >= right_max:
                    right_max = height[right]
                else:
                    total_water += right_max - height[right]
                right -= 1
        
        return total_water

```

17. Roman to integer
    Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

    Symbol       Value
    I             1
    V             5
    X             10
    L             50
    C             100
    D             500
    M             1000
    For example, 2 is written as II in Roman numeral, just two ones added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

    Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we         subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

    I can be placed before V (5) and X (10) to make 4 and 9. 
    X can be placed before L (50) and C (100) to make 40 and 90. 
    C can be placed before D (500) and M (1000) to make 400 and 900.
    Given a roman numeral, convert it to an integer.

```python

class Solution:
    def romanToInt(self, s: str) -> int:

        roman_to_int = { 
            'I': 1, 'V': 5, 'X': 10, 'L': 50, 
            'C': 100, 'D': 500, 'M': 1000
        }

        total = 0
        prev_value = 0

        for c in reversed(s):
            current_value = roman_to_int[c]

            if current_value < prev_value:
                total -= current_value
            else: 
                total += current_value

            prev_value = current_value
        
        return total

```

18. Integer to Roman
    
    Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.
    Given an integer, convert it to a Roman Numeral.

```python

class Solution:
    def intToRoman(self, num: int) -> str:
        roman_numerals = [
            (1000, 'M'), (900, 'CM'), (500, 'D'), (400, 'CD'),
            (100, 'C'), (90, 'XC'), (50, 'L'), (40, 'XL'),
            (10, 'X'), (9, 'IX'), (5, 'V'), (4, 'IV'), (1, 'I')
        ]

        result = []

        for value, symbol in roman_numerals:
            while num >= value:
                num -= value
                result.append(symbol)

        return ''.join(result)

```  

19. Length of Last Word
    
    Given a string s consisting of words and spaces, return the length of the last word in the string.
    A word is a maximal substring consisting of non-space characters only.

```python

class Solution:
    def lengthOfLastWord(self, s: str) -> int:

        # Strip leading and trailing spaces and remove punctuation at the end of the string
        # s = s.rstrip(string.punctuation)

        words = s.strip().split()

        return len(words[-1]) if words else 0

```

   But better to use loop on the interview

```python

class Solution:
    def lengthOfLastWord(self, s: str) -> int:

        end = len(s) - 1

        while s[end] == " ":
            end -= 1

        start = end
        
        while start >= 0 and s[start] != ' ':
            start -= 1
        
        return end - start

```

20. Longest prefix
    Write a function to find the longest common prefix string amongst an array of strings.
    If there is no common prefix, return an empty string "".

```python

class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        res = ''               # Initialize result as an empty string
        strs = sorted(strs)    # Sort the list of strings lexicographically

        for i in range(len(strs[0])):    # Loop through each character in the first string
            if strs[0][i] != strs[-1][i]:  # Compare character from the first and last string
                return res  # If mismatch occurs, return the current common prefix
        
            res += strs[0][i]   # Add the common character to the result
    
        return res   # Return the final longest common prefix
   
```

21. Reverse words in a String
    Given an input string s, reverse the order of the words.
    A word is defined as a sequence of non-space characters. The words in s will be separated by at least one space.
    Return a string of the words in reverse order concatenated by a single space.
    Note that s may contain leading or trailing spaces or multiple spaces between two words.
    The returned string should only have a single space separating the words. Do not include any extra spaces.


```python
# simple solution
# strip() remove leading and spaces
# split() create array of words
# [::-1] reverse array
# ' '.join join words into a sentence

class Solution:
    def reverseWords(self, s: str) -> str:
        new_string = ' '.join(s.strip().split()[::-1])

        return new_string
```

22. Zizag Conversion
    The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this:
    (you may want to display this pattern in a fixed font for better legibility)

    P   A   H   N
    A P L S I I G
    Y   I   R
    And then read line by line: "PAHNAPLSIIGYIR"

    Write the code that will take a string and make this conversion given a number of rows:

    string convert(string s, int numRows);

```python

def convert(s: str, numRows: int) -> str:
    if numRows == 1 or numRows >= len(s):
        return s  # No zigzag needed if only one row or string is too short
    
    # Create an array of strings for each row
    rows = [""] * numRows
    current_row = 0
    going_down = False
    
    # Traverse the string, placing each character in the appropriate row
    for char in s:
        rows[current_row] += char  # Add character to the current row
        # Change direction at the top or bottom of the zigzag
        if current_row == 0 or current_row == numRows - 1:
            going_down = not going_down
        # Move up or down
        current_row += 1 if going_down else -1
    
    # Combine all rows into one string
    return "".join(rows)

```
23. Find the Index of the First Occurence in a String
    
    Given two strings needle and haystack, return the index of the first
    occurrence of needle in haystack, or -1 if needle is not part of haystack.

```python

class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        return haystack.find(needle)

```
or 

```python

class Solution:
    def strStr(haystack: str, needle: str) -> int:
        # Check if the needle is an empty string
        if not needle:
            return 0  # The empty string is considered to be found at index 0 of any string
    
        # Loop through the haystack up to the point where the remaining characters are enough to compare with the needle
        for i in range(len(haystack) - len(needle) + 1):
            # Check if the substring of haystack starting at index i matches the needle
            if haystack[i:i + len(needle)] == needle:
                return i  # Return the starting index of the first occurrence of the needle
    
        # If no match is found, return -1
        return -1

```
