# 3Sum

## Solution
- Using two pointer, solution for this problem the same with 2Sum problem 

Python code

```python 
class Solution:
    def threeSum(self, nums: list[int]) -> list[list[int]]:
        # Sort the array to efficiently use the two-pointer technique
        nums.sort()
        
        result = []
        n = len(nums)
        
        # Iterate through the array with 'i' as the first element of the triplet
        for i in range(n - 2):
            # Skip duplicate elements for 'i' to avoid duplicate triplets in the result
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            
            # Initialize two pointers for the remaining part of the array
            left = i + 1
            right = n - 1
            
            # Use the two-pointer technique to find pairs that sum to -nums[i]
            while left < right:
                current_sum = nums[i] + nums[left] + nums[right]
                
                if current_sum == 0:
                    # Found a valid triplet
                    result.append([nums[i], nums[left], nums[right]])
                    
                    # Move both pointers to find other potential triplets
                    left += 1
                    right -= 1
                    
                    # Skip duplicate elements for 'left' and 'right'
                    # to avoid duplicate triplets
                    while left < right and nums[left] == nums[left - 1]:
                        left += 1
                    while left < right and nums[right] == nums[right + 1]:
                        right -= 1
                        
                elif current_sum < 0:
                    # Sum is too small, need a larger number
                    left += 1
                else: # current_sum > 0
                    # Sum is too large, need a smaller number
                    right -= 1
                    
        return result
        
```
