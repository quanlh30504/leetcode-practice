# String to Integer (atoi)

Implement the myAtoi(string s) function, which converts a string to a 32-bit signed integer.

The algorithm for myAtoi(string s) is as follows:

1. Whitespace: Ignore any leading whitespace (" ").
2. Signedness: Determine the sign by checking if the next character is '-' or '+', assuming positivity if neither present.
3. Conversion: Read the integer by skipping leading zeros until a non-digit character is encountered or the end of the string is reached. If no digits were read, then the result is 0.
4. Rounding: If the integer is out of the 32-bit signed integer range [-231, 231 - 1], then round the integer to remain in the range. Specifically, integers less than -231 should be rounded to -231, and integers greater than 231 - 1 should be rounded to 231 - 1.

Return the integer as the final result.

```python
class Solution:
    def myAtoi(self, s: str) -> int:
        # Giới hạn của số nguyên 32-bit
        INT_MAX = 2**31 - 1
        INT_MIN = -2**31

        # 1. Bỏ qua khoảng trắng ở đầu
        s = s.strip()

        # 2. Nếu chuỗi rỗng, trả về 0
        if not s:
            return 0

        # 3. Xử lý dấu (+/-)
        i = 0
        sign = 1  # Mặc định là số dương
        if s[i] == '+':
            i += 1
        elif s[i] == '-':
            sign = -1
            i += 1

        # 4. Chuyển đổi số và kiểm tra tràn số
        result = 0
        while i < len(s) and s[i].isdigit():
            digit = int(s[i])

            # --- ĐÂY LÀ PHẦN QUAN TRỌNG NHẤT ---
            # Kiểm tra tràn số TRƯỚC KHI thực hiện phép tính
            # Nếu result > INT_MAX / 10, thì result * 10 chắc chắn sẽ tràn
            if result > INT_MAX // 10 or (result == INT_MAX // 10 and digit > 7):
                return INT_MAX if sign == 1 else INT_MIN
            
            result = result * 10 + digit
            i += 1

        # 5. Trả về kết quả cuối cùng với dấu tương ứng
        return sign * result
```
