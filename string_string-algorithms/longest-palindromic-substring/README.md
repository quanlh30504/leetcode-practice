# Solution 

# Approach 1: Brute Force

## Intuition :
The obvious brute force solution is to pick all possible starting and ending positions for a substring, and verify if it is a palindrome. There are a total of n^2 such substrings (excluding the trivial solution where a character itself is a palindrome). Since verifying each substring takes O(n) time, the run time complexity is O(n^3).

## Algorithm :
1. Pick a starting index for the current substring which is every index from 0 to n-2.
2. Now, pick the ending index for the current substring which is every index from i+1 to n-1.
3. Check if the substring from ith index to jth index is a palindrome.
4. If step 3 is true and length of substring is greater than maximum length so far, update maximum length and maximum substring.
5. Print the maximum substring.

## Complexity Analysis
- Time complexity : O(n^3). Assume that n is the length of the input string, there are a total of C(n, 2) = n(n-1)/2 substrings (excluding the trivial solution where a character itself is a palindrome). 
Since verifying each substring takes O(n) time, the run time complexity is O(n^3).

- Space complexity : O(1).

### Python code
```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        if (len(s) <= 1) :
            return s
        max_len = 1
        max_str = s[0]

        for i in range(len(s)-1) :
            for j in range(len(s)) :
                if (j-i+1 > max_len and s[i:j+1] == s[i:j+1][::-1]) :
                    max_len = j-i+1
                    max_str = s[i:j+1]
        return max_str
```

# Approach 2: Expand Around Center

## Intuition :
To enumerate all palindromic substrings of a given string, we first expand a given string at each possible starting position of a palindrome and also at each possible ending position of a palindrome and keep track of the length of the longest palindrome we found so far.

## Approach :
We observe that a palindrome mirrors around its center. Therefore, a palindrome can be expanded from its center, and there are only 2n - 1 such centers.
You might be asking why there are 2n - 1 but not n centers? The reason is the center of a palindrome can be in between two letters. Such palindromes have even number of letters (such as "abba") and its center are between the two 'b's.'
Since expanding a palindrome around its center could take O(n) time, the overall complexity is O(n^2).
## Algorithm :
1. At starting we have maz_str = s[0] and max_len = 1 as every single character is a palindrome.
2. Now, we will iterate over the string and for every character we will expand around its center.
3. For odd length palindrome, we will consider the current character as the center and expand around it.
4. For even length palindrome, we will consider the current character and the next character as the center and expand around it.
4. We will keep track of the maximum length and the maximum substring.
5. Print the maximum substring.

### Complexity Analysis
- Time complexity : O(n^2). Since expanding a palindrome around its center could take O(n) time, the overall complexity is O(n^2).

- Space complexity : O(1).

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        if len(s) <= 1:
            return s

        def expand_from_center(left, right):
            while left >= 0 and right < len(s) and s[left] == s[right]:
                left -= 1
                right += 1
            return s[left + 1:right]

        max_str = s[0]

        for i in range(len(s) - 1):
            odd = expand_from_center(i, i)
            even = expand_from_center(i, i + 1)

            if len(odd) > len(max_str):
                max_str = odd
            if len(even) > len(max_str):
                max_str = even

        return max_str
```

# Approach 3: Manacher's Algorithm

## Intuition :
To avoid the unnecessary validation of palindromes, we can use Manacher's algorithm. The algorithm is explained brilliantly in this article. The idea is to use palindrome property to avoid unnecessary validations. We maintain a center and right boundary of a palindrome. We use previously calculated values to determine if we can expand around the center or not. If we can expand, we expand and update the center and right boundary. Otherwise, we move to the next character and repeat the process. We also maintain a variable max_len to keep track of the maximum length of the palindrome. We also maintain a variable max_str to keep track of the maximum substring.

## Algorithm :
1. We initialize a boolean table dp and mark all the values as false.
2. We will use a variable max_len to keep track of the maximum length of the palindrome.
3. We will iterate over the string and mark the diagonal elements as true as every single character is a palindrome.
4. Now, we will iterate over the string and for every character we will expand around its center.
5. For odd length palindrome, we will consider the current character as the center and expand around it.
6. For even length palindrome, we will consider the current character and the next character as the center and expand around it.
7. We will keep track of the maximum length and the maximum substring.
8. Print the maximum substring.
## Complexity Analysis
- Time complexity : O(n). Since expanding a palindrome around its center could take O(n) time, the overall complexity is O(n).

- Space complexity : O(n). It uses O(n) space to store the table.

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        if len(s) <= 1:
            return s
        
        Max_Len=1
        Max_Str=s[0]
        s = '#' + '#'.join(s) + '#'
        dp = [0 for _ in range(len(s))]
        center = 0
        right = 0
        for i in range(len(s)):
            if i < right:
                dp[i] = min(right-i, dp[2*center-i])
            while i-dp[i]-1 >= 0 and i+dp[i]+1 < len(s) and s[i-dp[i]-1] == s[i+dp[i]+1]:
                dp[i] += 1
            if i+dp[i] > right:
                center = i
                right = i+dp[i]
            if dp[i] > Max_Len:
                Max_Len = dp[i]
                Max_Str = s[i-dp[i]:i+dp[i]+1].replace('#','')
        return Max_Str
```
