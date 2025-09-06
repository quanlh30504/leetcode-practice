# Regular Expression Matching

Given an input string s and a pattern p, implement regular expression matching with support for '.' and '*' where:

- '.' Matches any single character.​​​​
- '*' Matches zero or more of the preceding element.
The matching should cover the <b>entire</b> input string (not partial).


Example 1:
```text
Input: s = "aa", p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```
Example 2:
```text
Input: s = "aa", p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```
Example 3:
```text
Input: s = "ab", p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
```

## Solution 

Using dynamic programing 

[Video explain code](https://www.youtube.com/watch?v=HAA8mgxlov8&t=1481s)

```python 
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        
        def dfs(i: int, j: int) -> bool:
            if i >= len(s) and j >= len(p) : return True

            if j >= len(p):
                return False

            match = i < len(s) and (s[i] == p[j] or p[j] == ".")

            if j + 1 < len(p) and p[j+1] == "*" :
                return dfs(i, j+2) or (match and dfs(i+1, j))

            if match :
                return dfs(i+1, j+1)
            return False

        return dfs(0,0)
```
