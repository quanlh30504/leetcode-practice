# Approach 1: Split and Compare
## Intuition
Ý tưởng chính là coi mỗi chuỗi phiên bản như một dãy các con số (revision). Ta có thể tách chuỗi bằng dấu chấm . để lấy ra các con số này. Sau đó, ta so sánh từng con số tương ứng ở mỗi vị trí từ trái sang phải. Để xử lý các phiên bản có số lượng revision khác nhau (ví dụ: "1.0" và "1.0.1"), ta có thể coi như phiên bản ngắn hơn được thêm các số 0 ở cuối (ví dụ: "1.0" tương đương với "1.0.0"). Ta sẽ duyệt qua tất cả các vị trí cho đến hết phiên bản dài hơn, coi các vị trí còn thiếu ở phiên bản ngắn hơn là 0.

## Algorithm
1. Tách chuỗi version1 và version2 bằng dấu chấm .. Chuyển đổi mỗi phần thành số nguyên và lưu vào hai danh sách tương ứng là v1 và v2.

2. Tìm độ dài lớn nhất (len_max) giữa hai danh sách v1 và v2.

3. Lặp qua các chỉ số i từ 0 đến len_max - 1.

4. Trong mỗi vòng lặp:

- Nếu i vượt ra ngoài độ dài của v1: Điều này có nghĩa version1 ngắn hơn. Ta chỉ cần kiểm tra xem các số còn lại trong v2 (tức là v2[i]) có lớn hơn 0 không. Nếu có, version2 lớn hơn, trả về -1.

- Nếu i vượt ra ngoài độ dài của v2: Tương tự, version2 ngắn hơn. Ta kiểm tra các số còn lại trong v1 (tức là v1[i]). Nếu lớn hơn 0, version1 lớn hơn, trả về 1.

- Nếu i nằm trong phạm vi của cả hai: So sánh trực tiếp v1[i] và v2[i].

- Nếu v1[i] < v2[i], trả về -1.

- Nếu v1[i] > v2[i], trả về 1.

5. Nếu vòng lặp kết thúc mà không trả về giá trị nào, điều đó có nghĩa là tất cả các phần so sánh đều bằng nhau. Hai phiên bản này là tương đương, trả về 0.

## Complexity Analysis
Time complexity: O(max(N,M))

Trong đó N và M lần lượt là số lượng revision (các số được ngăn cách bởi dấu chấm) trong version1 và version2.

Thao tác split và map mất thời gian tuyến tính theo độ dài của chuỗi. Vòng lặp chính chạy 
max(N,M) lần. Vì vậy, độ phức tạp tổng thể phụ thuộc vào số lượng revision lớn nhất.

Space complexity: O(N+M)


```python

class Solution:
    def compareVersion(self, version1: str, version2: str) -> int:
        v1 = list(map(int,version1.split('.')))
        v2 = list(map(int,version2.split('.')))

        len_max = max(len(v1), len(v2))
        for i in range(len_max) :
            if (i >= len(v1)) : 
                if (v2[i] > 0) : return -1
                continue
            if (i >= len(v2)) :
                if v1[i] > 0 : return 1
                continue
            if v1[i] < v2[i] : return -1
            elif v1[i] > v2[i] : return 1
        return 0
```
