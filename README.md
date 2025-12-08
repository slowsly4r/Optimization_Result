# PHÂN TÍCH HIỆU NĂNG: TỐI ƯU HOÁ TRÊN TẬP KHẢ ĐẠT (Optimization over Reachable Markings)

## 📊 GIỚI THIỆU

Dự án này thực hiện đánh giá hiệu suất của thuật toán **Tìm kiếm toàn bộ trên BDD (Enumeration over BDD)** để giải quyết bài toán tối ưu hóa trong Mạng Petri.

* **Mục tiêu:** Tìm kiếm một trạng thái $M$ nằm trong tập khả đạt $R(M_0)$ sao cho hàm mục tiêu tuyến tính $f(M) = c^T \cdot M$ đạt giá trị lớn nhất.
* **Phương pháp tiếp cận:**
    1.  **BDD Construction:** Biểu diễn tập khả đạt bằng cấu trúc BDD từ task trước (sử dụng `bdd_reachable`).
    2.  **Enumeration-Based Optimization:** Duyệt qua tất cả các marking khả đạt bằng cách sử dụng iterator `pick_iter` trên BDD với các biến liên quan (places). Đối với mỗi marking, tính giá trị mục tiêu tuyến tính và chọn marking có giá trị lớn nhất.

## 📁 DỮ LIỆU KIỂM THỬ

Hệ thống được kiểm thử trên **15 bộ dữ liệu (datasets)**, bao gồm:

* **Mạng cơ bản:** `input1.pnml` đến `input6.pnml` (kiểm tra tính đúng đắn).
* **Mạng Benchmark (Cấu trúc đặc biệt):**
    * `input7` (Mixed Stress), `input8` (Parallel), `input10` (Ring).
    * `input9` (Read-Arc), `input11` (Self-loop), `input12` (Source-Sink).
* **Mạng mở rộng (Scalability Tests):** `input13`, `input14`, `input15` (kiểm tra khả năng chịu tải với không gian trạng thái lớn).

## 📈 KẾT QUẢ THỰC NGHIỆM

Dưới đây là tổng hợp chi tiết kết quả chạy thực tế của thuật toán tối ưu hóa. Các marking được liệt kê đầy đủ từ output thực tế.

### 1. Nhóm mạng cơ bản
*Các mạng nhỏ dùng để kiểm tra logic.*

* **`input1.pnml`** ($|P|=4, |T|=2$)
    * Max Value: **2** | Time: `0.0000s` | Mem: `0.0059 MB`
    * Marking: `[0, 1, 1, 0]`

* **`input2.pnml`** ($|P|=5, |T|=4$)
    * Max Value: **3** | Time: `0.0000s` | Mem: `0.0076 MB`
    * Marking: `[1, 0, 1, 0, 1]`

* **`input3.pnml`** ($|P|=4, |T|=2$)
    * Max Value: **3** | Time: `0.0000s` | Mem: `0.0050 MB`
    * Marking: `[1, 1, 0, 1]`

* **`input4.pnml`** ($|P|=23, |T|=23$)
    * Max Value: **11** | Time: `0.2860s` | Mem: `0.0392 MB`
    * Marking: `[0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 0, 1]` (Xen kẽ)

* **`input5.pnml`** ($|P|=8, |T|=6$)
    * Max Value: **4** | Time: `0.0000s` | Mem: `0.0105 MB`
    * Marking: `[1, 0, 0, 1, 0, 0, 1, 1]`

* **`input6.pnml`** ($|P|=8, |T|=12$)
    * Max Value: **4** | Time: `0.0000s` | Mem: `0.0105 MB`
    * Marking: `[0, 0, 0, 0, 1, 1, 1, 1]`

---

### 2. Nhóm mạng Benchmarks (Cấu trúc đặc biệt)

> **Input 7 (Mixed Stress Model)**  
> * Quy mô: $|P|=15, |T|=20$  
> * **Max Value: 11** | Time: `0.0702s` | Mem: `0.0117 MB`  
> * Marking: `[0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0]`

> **Input 8 (Parallel Model)**  
> * Quy mô: $|P|=12, |T|=12$  
> * **Max Value: 12** | Time: `0.1700s` | Mem: `0.0038 MB`  
> * Marking: `[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]` (Full active)

> **Input 9 (Read-Arc Model)**  
> * Quy mô: $|P|=8, |T|=10$  
> * **Max Value: 8** | Time: `0.0000s` | Mem: `0.0084 MB`  
> * Marking: `[1, 1, 1, 1, 1, 1, 1, 1]`

> **Input 10 (Ring Model)**  
> * Quy mô: $|P|=8, |T|=8$  
> * **Max Value: 1** | Time: `0.0000s` | Mem: `0.0106 MB`  
> * Marking: `[0, 0, 0, 0, 0, 0, 0, 1]`  
> * *Nhận xét:* Invariant được bảo toàn tuyệt đối (chỉ 1 token trong mạng).

> **Input 11 (Self-loop Model)**  
> * Quy mô: $|P|=10, |T|=6$  
> * **Max Value: 6** | Time: `0.0000s` | Mem: `0.0079 MB`  
> * Marking: `[1, 0, 1, 1, 1, 1, 1, 0, 0, 0]`

> **Input 12 (Source-Sink Model)**  
> * Quy mô: $|P|=12, |T|=16$  
> * **Max Value: 8** | Time: `0.0120s` | Mem: `0.0064 MB`  
> * Marking: `[1, 1, 1, 0, 1, 1, 0, 0, 0, 1, 1, 1]`

---

### 3. Nhóm mạng Scalability (Kiểm thử chịu tải)
*Nhóm mạng kích thước lớn, thời gian xử lý tăng theo hàm mũ hoặc tuyến tính tùy cấu trúc.*

* **`input13.pnml`** ($|P|=28, |T|=21$)
    * **Max Value: 7** | Time: `1.4902s` | Mem: `0.0521 MB`
    * Marking: `[0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1]` (Mẫu lặp xen kẽ)

* **`input14.pnml`** ($|P|=32, |T|=24$)
    * **Max Value: 8** | Time: `8.2240s` | Mem: `0.0622 MB`
    * Marking: `[0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1]` (Mẫu lặp xen kẽ)

* **`input15.pnml`** ($|P|=36, |T|=27$)
    * **Max Value: 9** | Time: `34.3481s` | Mem: `0.0724 MB`
    * Marking: `[0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1]` (Mẫu lặp xen kẽ)

## 💡 PHÂN TÍCH & KẾT LUẬN

1.  **Hiệu năng:** Thuật toán xử lý tức thời (< 0.01s) với các mạng dưới 20 places nhờ enumeration hiệu quả trên BDD. Với các mạng lớn (input15), thời gian tăng lên (~34s) do số lượng marking khả đạt lớn, nhưng vẫn khả thi cho scalability tests.
2.  **Bộ nhớ (Memory Efficient):** Ưu điểm lớn nhất là khả năng nén trạng thái của BDD. Ngay cả khi xử lý các mạng lớn, bộ nhớ sử dụng chưa bao giờ vượt quá 0.1 MB.
3.  **Độ chính xác:** Kết quả Max Value và Marking phản ánh đúng các ràng buộc (ví dụ: Input 10 Ring model luôn có Max = 1). Tổng thời gian chạy toàn bộ tests: **45.52s**.