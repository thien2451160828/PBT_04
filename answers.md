### Câu A1:

**Bảng phân biệt 5 loại Positioning:**

| Position | Vẫn chiếm chỗ trong flow? | Tham chiếu vị trí (Mốc căn chỉnh) | Cuộn theo trang? | Use case (Ứng dụng thực tế) |
| :--- | :--- | :--- | :--- | :--- |
| `static` | Có | Luồng tài liệu mặc định (Normal document flow) | Có | Là giá trị mặc định. Dùng để sắp xếp các phần tử HTML tuần tự bình thường từ trên xuống dưới. |
| `relative` | Có | Vị trí ban đầu của chính phần tử đó | Có | Dịch chuyển nhẹ phần tử, thường dùng để làm "mốc tọa độ" (container) cho phần tử con `absolute`. |
| `absolute` | Không | Phần tử tổ tiên gần nhất có position khác `static` | Có (cuộn theo phần tử cha chứa nó) | Tạo các thành phần nổi đè lên phần tử khác như: Dropdown menu, Tooltip, Badge "HOT", Icon (X). |
| `fixed` | Không | Cửa sổ trình duyệt (Viewport) | Không (Gắn chặt vào một vị trí trên màn hình) | Thanh điều hướng (Navbar) ghim trên cùng, nút "Back to top", nút Chat ở góc dưới màn hình. |
| `sticky` | Có | Kết hợp giữa `relative` (trong vùng chứa) và `fixed` (khi chạm ngưỡng) | Có (cho đến khi chạm ngưỡng top/bottom thì sẽ "dính" lại) | Sidebar ghim trên màn hình khi cuộn, hoặc tiêu đề nhóm dính lại trong danh sách dài. |

**Trả lời câu hỏi thêm:**
* **Khi nào `absolute` tham chiếu `body`?** Khi tất cả các phần tử tổ tiên bọc ngoài nó (cha, ông...) đều không có thuộc tính `position`, tức là tất cả đều đang mang giá trị `static` mặc định.
* **Khi nào tham chiếu parent?** Khi phần tử cha trực tiếp được chủ động thiết lập một thuộc tính `position` mang giá trị khác `static` (thường là `position: relative;`). Lúc này phần tử con `absolute` sẽ di chuyển tự do bên trong không gian của phần tử cha đó.
* **Giải thích khái niệm "nearest positioned ancestor":** Dịch ra là "phần tử tổ tiên được định vị gần nhất". Khi gán `position: absolute`, trình duyệt sẽ chạy ngược lên trên cấu trúc HTML để dò tìm phần tử bọc ngoài nó. Phần tử **đầu tiên** bắt gặp có chứa thuộc tính `position` (khác `static`) sẽ lập tức được chọn làm "khung tọa độ gốc" (điểm 0,0) cho phần tử `absolute` đó dựa vào để căn chỉnh `top`, `right`, `bottom`, `left`.

### Câu A2:

**Trường hợp 1: Flexbox cơ bản (4 items)**
* **Dự đoán:** 4 items sẽ nằm trên cùng 1 hàng ngang (do mặc định là `flex-direction: row`) và chia đều không gian của container (mỗi item chiếm 25% chiều rộng do `flex: 1`).
* **Sơ đồ bố cục (1 hàng x 4 cột đều nhau):**
  ┌────────┬────────┬────────┬────────┐
  │ Item 1 │ Item 2 │ Item 3 │ Item 4 │
  └────────┴────────┴────────┴────────┘

**Trường hợp 2: Flexbox Wrap (6 items)**
* **Dự đoán:** Mỗi item chiếm `45% width` + `2 * 2.5% margin` = 50% không gian. Vì container có `flex-wrap: wrap`, nên mỗi hàng chỉ chứa được tối đa 2 items. Với 6 items, layout sẽ bẻ thành 3 hàng, mỗi hàng 2 items.
* **Sơ đồ bố cục (3 hàng x 2 cột):**
  ┌──────────────┐ ┌──────────────┐
  │    Item 1    │ │    Item 2    │
  └──────────────┘ └──────────────┘
  ┌──────────────┐ ┌──────────────┐
  │    Item 3    │ │    Item 4    │
  └──────────────┘ └──────────────┘
  ┌──────────────┐ ┌──────────────┐
  │    Item 5    │ │    Item 6    │
  └──────────────┘ └──────────────┘

**Trường hợp 3: Flexbox Space-between & Center (3 items)**
* **Dự đoán:** 3 items nằm trên 1 hàng ngang và được căn giữa theo trục dọc (`align-items: center`). Trục ngang sử dụng `space-between` nên Item 1 sẽ bám sát lề trái, Item 3 bám sát lề phải, và Item 2 nằm chính giữa, tạo ra khoảng trống đều nhau ở giữa các items.
* **Sơ đồ bố cục:**
  ┌──────┐                            ┌──────┐                            ┌──────┐
  │Item 1│                            │Item 2│                            │Item 3│
  └──────┘                            └──────┘                            └──────┘

**Trường hợp 4: Grid Layout (3 items)**
* **Dự đoán:** Lưới gồm 1 hàng và 3 cột. Cột đầu và cột cuối có chiều rộng cố định là 200px. Cột giữa sẽ tự động co giãn (`1fr`) để lấp đầy phần không gian còn lại của container. Ở giữa các items có khoảng hở 20px (`gap`).
* **Sơ đồ bố cục:**
  [200px]      [        1fr (flexible)        ]      [200px]
  ┌─────┐      ┌──────────────────────────────┐      ┌─────┐
  │ It 1│      │            Item 2            │      │ It 3│
  └─────┘      └──────────────────────────────┘      └─────┘

**Trường hợp 5: Grid Layout Wrap (7 items)**
* **Dự đoán:** Lưới định nghĩa 3 cột có chiều rộng bằng nhau (mỗi cột `1fr`). Khi có 7 items, Grid sẽ tự động đưa các items lấp đầy từ trái sang phải, cứ đủ 3 items thì tự động xuống hàng. Kết quả tạo ra 3 hàng: hàng 1 có 3 items, hàng 2 có 3 items. Item cuối cùng (số 7) sẽ bị rớt xuống đầu hàng thứ 3.
* **Sơ đồ bố cục (3 hàng x 3 cột):**
  ┌───────┐  ┌───────┐  ┌───────┐
  │Item 1 │  │Item 2 │  │Item 3 │
  └───────┘  └───────┘  └───────┘
  ┌───────┐  ┌───────┐  ┌───────┐
  │Item 4 │  │Item 5 │  │Item 6 │
  └───────┘  └───────┘  └───────┘
  ┌───────┐  [Trống]    [Trống]
  │Item 7 │  
  └───────┘

## PHẦN C — SUY LUẬN (20 điểm)

### Câu C1 (10đ) — Flexbox vs Grid: Khi nào dùng gì?

1. **Navigation bar ngang (logo + menu + buttons):**
   - **Dùng:** Flexbox
   - **Giải thích:** Navbar là bố cục 1 chiều (1D - nằm ngang). Flexbox sinh ra để xử lý việc căn chỉnh, giãn cách (`space-between`, `center`) các phần tử trên cùng một hàng rất dễ dàng và linh hoạt.

2. **Lưới ảnh Instagram (3 cột đều nhau, số ảnh không biết trước):**
   - **Dùng:** Grid
   - **Giải thích:** Đây là bố cục 2 chiều (2D - gồm cả hàng và cột). Grid giúp kiểm soát kích thước cột và khoảng cách (`gap`) cực kỳ chuẩn xác, các ảnh sẽ tự động xếp vào lưới mà không bị so le dù số lượng ảnh thay đổi.

3. **Layout blog: main content + sidebar:**
   - **Dùng:** Kết hợp cả hai (hoặc thiên về Grid)
   - **Giải thích:** CSS Grid rất lý tưởng cho layout tổng thể (Macro-layout) để chia tỷ lệ trang (ví dụ: `grid-template-columns: 3fr 1fr`). Sau đó, bên trong các bài viết (main content), có thể dùng Flexbox để dàn xếp các chi tiết nhỏ như thẻ tag, ngày tháng, nút bấm.

4. **Footer với 4 cột thông tin (Về chúng tôi, Liên kết, Hỗ trợ, Liên hệ):**
   - **Dùng:** Grid
   - **Giải thích:** Việc chia thành 4 cột đều nhau với Grid cực kỳ ngắn gọn, chỉ cần 1 dòng code `grid-template-columns: repeat(4, 1fr)`. Nếu dùng Flexbox sẽ phải tính toán % width cho từng cột phức tạp hơn.

5. **Card sản phẩm (ảnh trên, text giữa, nút dưới — nút luôn dính đáy):**
   - **Dùng:** Flexbox
   - **Giải thích:** Đây là bố cục 1 chiều dọc. Bằng cách thiết lập `.card` là `flex-direction: column`, ta có thể sử dụng kỹ thuật `margin-top: auto` cho nút bấm để ép nó luôn dính vào đáy card một cách hoàn hảo.

---

### Câu C2 (10đ) — Debug Flexbox

**Lỗi 1: Cards không đều chiều cao — nút "Mua" bị nhảy lên/xuống**
- **Nguyên nhân:** Mặc định các nội dung bên trong card có thể dài ngắn khác nhau. Để quản lý vị trí của nút "Mua", bản thân thẻ `.card` cần phải trở thành một Flex container hướng dọc, từ đó mới đẩy được nút bấm xuống dưới.
- **Code sửa:**
  ```css
  .card { 
      width: 30%; 
      margin: 1.5%; 
      display: flex;             
      flex-direction: column;   
  }
  .card img { width: 100%; }
  .card h3 { font-size: 18px; }
  .card .btn { 
      padding: 10px; 
      margin-top: auto;          
  }
**Lỗi 2: Muốn items nằm giữa ngang lẫn dọc trong 100vh**

**Nguyên nhân:** Class .hero mới chỉ khai báo hộp chứa là Flex chứ chưa định nghĩa cách căn chỉnh bên trong. Thuộc tính text-align: center chỉ căn giữa chữ, không thể căn giữa cả một khối phần tử theo cả 2 trục.

Code sửa:

CSS
.hero {
    height: 100vh;
    display: flex;
    justify-content: center;   /* Căn giữa theo trục ngang */
    align-items: center;       /* Căn giữa theo trục dọc */
}
.hero-content {
    text-align: center;
}

**Lỗi 3:** Sidebar bị co lại khi content quá dài

**Nguyên nhân:** Các phần tử con trong Flexbox có tính chất mặc định là flex-shrink: 1, nghĩa là chúng sẽ tự động bị bóp nhỏ lại để nhường chỗ nếu vùng chứa bị chật.

Code sửa:

CSS
.layout { display: flex; }
.sidebar { 
    width: 250px; 
    flex-shrink: 0;            /* Thêm dòng này để cấm sidebar bị thu nhỏ */
}
.content { flex: 1; }

