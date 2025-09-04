# Longest Substring Without Repeating Characters

Given a string s, find the length of the longest substring without duplicate characters.


**Example 1:**
```text 
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```
**Example 2:**
```text 
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```
**Example 3:**
```text 
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

# Approach
We have two conditions to solve this question. The longest string should be

- Substring
- Without repeating characters

You just need to check that there are no repeated characters within a consecutive string of characters. To achieve this, you need to keep track of the characters currently forming the string. For this purpose, I considered three algorithms: one combining a sliding window and a set, and the second using a sliding window and a hash-based algorithm. The thrid is that the last position where each character was seen.

I'll explain them one by one.

# Solution 1: Sliding Window & Set
Giải pháp này sử dụng kỹ thuật Sliding Window kết hợp với một Set để theo dõi các ký tự duy nhất trong cửa sổ hiện tại.

### Ý tưởng
Chúng ta sử dụng hai con trỏ, left và right, để định nghĩa một "cửa sổ trượt" trên chuỗi. Con trỏ right sẽ di chuyển qua chuỗi, mở rộng cửa sổ. Chúng ta sử dụng một set (char_set) để lưu trữ các ký tự trong cửa sổ hiện tại.

Khi right di chuyển và gặp một ký tự mới chưa có trong char_set, chúng ta thêm nó vào set và cập nhật độ dài lớn nhất.

Khi right gặp một ký tự đã có trong char_set, chúng ta biết rằng có một ký tự lặp lại. Để loại bỏ sự lặp lại, chúng ta phải thu hẹp cửa sổ bằng cách di chuyển con trỏ left về phía trước. Chúng ta sẽ xóa các ký tự khỏi char_set và tăng left cho đến khi ký tự lặp lại không còn trong cửa sổ.

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        left = max_length = 0
        char_set = set()
        
        for right in range(len(s)):
            while s[right] in char_set:
                char_set.remove(s[left])
                left += 1

            char_set.add(s[right])
            max_length = max(max_length, right - left + 1)
        
        return max_length
```
### Phân tích độ phức tạp
- Thời gian: O(n). Mặc dù có vòng lặp while lồng nhau, cả hai con trỏ left và right chỉ di chuyển qua chuỗi một lần duy nhất.

- Không gian: O(1). Do các ký tự là cố định (chuỗi ASCII), kích thước của set không vượt quá một hằng số.

# Solution 2: Sliding Window & Hashing
Giải pháp này cũng sử dụng Sliding Window, nhưng thay vì set, chúng ta dùng một dictionary (hash map) để lưu trữ tần suất của các ký tự.

### Ý tưởng
Sử dụng hai con trỏ left và right.

Một dictionary (count) sẽ lưu trữ số lần xuất hiện của mỗi ký tự trong cửa sổ.

Khi right di chuyển, chúng ta tăng tần suất của ký tự hiện tại trong count.

Nếu tần suất của ký tự hiện tại lớn hơn 1, nghĩa là có sự lặp lại. Chúng ta dùng một vòng lặp while để giảm tần suất của ký tự ở vị trí left và tăng left lên cho đến khi sự lặp lại được loại bỏ.

Sau đó, cập nhật độ dài lớn nhất.

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        max_length = left = 0
        count = {}

        for right, c in enumerate(s):
            count[c] = 1 + count.get(c, 0)
            while count[c] > 1:
                count[s[left]] -= 1
                left += 1
        
            max_length = max(max_length, right - left + 1)

        return max_length
```
### Phân tích độ phức tạp
- Thời gian: O(n).

- Không gian: O(1).

# Solution 3: Sliding Window với vị trí cuối cùng của ký tự
Đây là một biến thể tối ưu hơn của Sliding Window. Thay vì xóa các ký tự, chúng ta trực tiếp nhảy con trỏ left đến đúng vị trí cần thiết.

### Ý tưởng
Sử dụng một dictionary (last_seen) để lưu trữ chỉ mục (index) cuối cùng của mỗi ký tự đã được thấy.

Khi right di chuyển, chúng ta kiểm tra xem ký tự hiện tại (c) đã từng xuất hiện trước đó chưa và chỉ mục của nó (last_seen[c]) có nằm trong cửa sổ hiện tại (lớn hơn hoặc bằng left) không.

Nếu có, chúng ta cập nhật left để nó nhảy đến vị trí last_seen[c] + 1, bỏ qua tất cả các ký tự trước đó, bao gồm cả ký tự lặp lại.

Sau đó, chúng ta cập nhật last_seen[c] thành chỉ mục right hiện tại và cập nhật độ dài lớn nhất.

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        max_length = 0
        left = 0
        last_seen = {}

        for right, c in enumerate(s):
            if c in last_seen and last_seen[c] >= left:
                left = last_seen[c] + 1
        
            max_length = max(max_length, right - left + 1)
            last_seen[c] = right

        return max_length
```
### Phân tích độ phức tạp
- Thời gian: O(n). Vòng lặp chỉ chạy một lần duy nhất.

- Không gian: O(1). Tương tự như các giải pháp trên.
