# Largest Number

Given a list of non-negative integers nums, arrange them such that they form the largest number and return it.

Since the result may be very large, so you need to return a string instead of an integer.

 

Example 1:
```text
Input: nums = [10,2]
Output: "210"
```
Example 2:
```text
Input: nums = [3,30,34,5,9]
Output: "9534330"
```


## Solution 1: Brute - force 
My code
```python
class Solution:

    # return 1 -> choose str1, return -1 -> choose str2, return 0 -> 2 object the same
    def chooseObject(self, str1: str, str2: str) -> int:
        i = j = 0
        while i < len(str1) and j < len(str2):
            if int(str1[i]) < int(str2[j]):
                return -1
            elif int(str1[i]) > int(str2[j]):
                return 1
            else:
                i += 1
                j += 1

        if i < len(str1) or j < len(str2):
            intVer1 = int(str1 + str2)
            intVer2 = int(str2 + str1)
            if intVer1 > intVer2:
                return 1
            return -1
        return 0

    def largestNumber(self, nums: list[int]) -> str:
        strNum = []
        for i in range(len(nums)):
            strNum.append(str(nums[i]))

        for i in range(0, len(strNum) - 1):
            selectIndex = i
            for j in range(i, len(strNum)):
                check = self.chooseObject(strNum[selectIndex], strNum[j])
                if check == -1:
                    selectIndex = j

            if selectIndex != i:
                tmp = strNum[i]
                strNum[i] = strNum[selectIndex]
                strNum[selectIndex] = tmp
        result = "".join(strNum)
        if int(result) == 0 : return "0"
        return result

```

## Solution 2: Greedy 

```python 
from functools import cmp_to_key

class Solution:
    def largestNumber(self, nums: list[int]) -> str:
        strNum = []
        for i in range(len(nums)):
            strNum.append(str(nums[i]))

        def compare (o1: str, o2: str) -> int :
            if o1 + o2 > o2 + o1 : return -1
            else: return 1

        nums = sorted(strNum, key=cmp_to_key(compare))

        return str(int("".join(nums)))
```
