Use Case Name & ID | Đặt hàng Online (UC-01)
Actor(s) & Description | Khách hàng: Người dùng ứng dụng RikkeiFastMart muốn mua sản phẩm. Hệ thống: Ứng dụng RikkeiFastMart xử lý các yêu cầu đặt hàng.
Pre-conditions | - Khách hàng đã đăng nhập vào ứng dụng. - Giỏ hàng của Khách hàng có ít nhất một sản phẩm. - Kết nối mạng ổn định.
Post-conditions | - Đơn hàng mới được tạo thành công trong hệ thống. - Số lượng tồn kho của các Sản phẩm trong Đơn hàng được cập nhật giảm đi. - Khách hàng nhận được thông báo đặt hàng thành công.
Main Flow | 1. Khách hàng bấm nút "Đặt hàng" trên giỏ hàng. 2. Hệ thống thực hiện kiểm tra tồn kho các Sản phẩm trong Giỏ hàng (UC-01a: Kiểm tra tồn kho). 3. Hệ thống hiển thị màn hình xác nhận đơn hàng với tổng tiền và tùy chọn nhập Mã giảm giá. 4. Khách hàng nhập Mã giảm giá (nếu có) và bấm "Áp dụng" (UC-01b: Áp dụng mã giảm giá). 5. Hệ thống hiển thị lại tổng tiền sau khi áp dụng Mã giảm giá (nếu thành công). 6. Khách hàng bấm nút "Xác nhận đặt hàng". 7. Hệ thống tạo mới một Đơn hàng với trạng thái "Đã đặt", lưu thông tin Đơn hàng và Chi tiết Đơn hàng, đồng thời cập nhật giảm số lượng tồn kho của các Sản phẩm tương ứng. 8. Hệ thống gửi thông báo "Đặt hàng thành công" cho Khách hàng.
Alternative Flows | **UC-01a: Kiểm tra tồn kho thất bại (tại bước 2)**: 2a. Nếu một hoặc nhiều Sản phẩm trong Giỏ hàng không đủ số lượng tồn kho: 2a1. Hệ thống hiển thị thông báo "Sản phẩm [Tên sản phẩm] chỉ còn [Số lượng tồn kho] sản phẩm. Vui lòng cập nhật giỏ hàng." 2a2. Hệ thống không tạo đơn hàng. 2a3. Luồng kết thúc. **UC-01b: Áp dụng mã giảm giá không hợp lệ (tại bước 4)**: 4a. Nếu Mã giảm giá không tồn tại, đã hết hạn, hoặc không hợp lệ: 4a1. Hệ thống hiển thị thông báo "Mã giảm giá không hợp lệ hoặc đã hết hạn." 4a2. Hệ thống không áp dụng giảm giá. 4a3. Khách hàng có thể nhập mã khác hoặc bỏ qua bước này. 4a4. Luồng tiếp tục tại bước 6 (Main Flow).

*Giải thích:* Đặc tả Use Case "Đặt hàng Online" mô tả chi tiết các bước trong luồng chính và các luồng thay thế, giúp các bên liên quan hiểu rõ hành vi của hệ thống trong các tình huống khác nhau. Luồng thay thế xử lý các trường hợp sản phẩm không đủ tồn kho hoặc mã giảm giá không hợp lệ, đảm bảo tính đầy đủ của nghiệp vụ.

#### Activity Diagram cho Use Case Đặt hàng Online

Activity Diagram thể hiện trực quan các bước trong Main Flow và các điểm rẽ nhánh của Alternative Flows. Các Swimlane được chia rõ ràng theo Actor và Hệ thống để theo dõi luồng xử lý.

[Link tới sơ đồ Activity Diagram](activity_diagram.drawio)

### Bước 5 — Class Diagram

Dựa trên kỹ thuật Danh từ – Động từ từ Main Flow ở Bước 4 và "Dữ liệu cần lưu" ở mục 2, tôi đã chọn ra 3 Class chính: `Khách hàng`, `Sản phẩm` và `Đơn hàng`. Để thể hiện mối quan hệ nhiều-nhiều giữa `Đơn hàng` và `Sản phẩm` kèm theo thuộc tính `số lượng mua của từng sản phẩm trong đơn`, một Association Class `Chi tiết Đơn hàng` sẽ được sử dụng trong sơ đồ Class Diagram.

| Class           | Thuộc tính                                          | Phương thức                                                                            |
| :-------------- | :-------------------------------------------------- | :------------------------------------------------------------------------------------- |
| `Khách hàng`    | `+id: int`, `+hoTen: string`, `+email: string`      | `+dangNhap(email, matKhau): bool`, `+datHang(): void`                                  |
| `Sản phẩm`      | `+id: int`, `+tenSP: string`, `+donGia: double`, `+soLuongTonKho: int` | `+kiemTraTonKho(soLuongYeuCau: int): bool`, `+capNhatTonKho(soLuong: int): void`        |
| `Đơn hàng`      | `+id: int`, `+ngayDat: Date`, `+maGiamGia: string?` | `+themChiTietDonHang(sanPham: SanPham, soLuong: int): void`, `+tinhTongTien(): double`, `+taoDonHang(): void` |

*Giải thích về quan hệ Composition/Aggregation:*

Quan hệ giữa `Đơn hàng` và `Chi tiết Đơn hàng` là **Composition (Hợp thành)**. Một `Chi tiết Đơn hàng` (ví dụ: một dòng hàng cụ thể trong đơn hàng với số lượng và sản phẩm) không thể tồn tại độc lập mà phải thuộc về một `Đơn hàng` cụ thể. Khi một `Đơn hàng` bị xóa, tất cả các `Chi tiết Đơn hàng` liên quan đến nó cũng sẽ bị xóa theo. Điều này thể hiện sự phụ thuộc tồn tại mạnh mẽ và sự sở hữu độc quyền của `Đơn hàng` đối với các `Chi tiết Đơn hàng` của nó.

[Link tới sơ đồ Class Diagram](class_diagram.drawio)

### Bước 6 — Sequence Diagram

Sequence Diagram minh họa luồng tương tác giữa Actor chính (`Khách hàng`) và các đối tượng hệ thống (được đại diện bởi các Lifeline) cho Main Flow của Use Case "Đặt hàng Online". Nó thể hiện thứ tự các thông điệp được gửi và nhận, cùng với thời gian kích hoạt của các đối tượng.

[Link tới sơ đồ Sequence Diagram](sequence_diagram.drawio)

### Bước 7 — UI Wireframe

Phác thảo một màn hình giao diện người dùng xuất hiện trong Main Flow của luồng "Đặt hàng Online", cụ thể là màn hình "Xác nhận Đơn hàng". Màn hình này tương ứng với các bước 3, 4, 5, 6 của Main Flow.

**Các thành phần UI cơ bản và khớp với bước Main Flow:**

1.  **Text Label hiển thị "Tổng tiền tạm tính: [Số tiền]"**: Xuất hiện ở **bước 3** Main Flow ("Hệ thống hiển thị màn hình xác nhận đơn hàng với tổng tiền...").
2.  **Input Field "Mã giảm giá"**: Cho phép Khách hàng nhập mã, tương ứng với **bước 4** Main Flow ("Khách hàng nhập Mã giảm giá...").
3.  **Button "Xác nhận Đặt hàng"**: Nút bấm cuối cùng để hoàn tất quá trình đặt hàng, tương ứng với **bước 6** Main Flow ("Khách hàng bấm nút 'Xác nhận đặt hàng'").

[Link tới sơ đồ UI Wireframe](ui_wireframe.drawio)

### Bước 8 — ERD

ERD (Entity-Relationship Diagram) chuyển đổi các Class ở Bước 5 thành các bảng cơ sở dữ liệu và thể hiện mối quan hệ giữa chúng, kèm theo quy tắc ánh xạ (Class → Entity, Attribute → Column, Relationship → Khóa ngoại hoặc bảng trung gian).

| Bảng                | Cột (ghi rõ PK/FK)                                                                         |
| :------------------ | :----------------------------------------------------------------------------------------- |
| `Khach_hang`        | `id (PK)`, `ho_ten`, `email`                                                               |
| `San_pham`          | `id (PK)`, `ten_sp`, `don_gia`, `so_luong_ton_kho`                                         |
| `Don_hang`          | `id (PK)`, `ngay_dat`, `ma_giam_gia`, `khach_hang_id (FK)`                                 |
| `Chi_tiet_Don_hang` | `don_hang_id (PK, FK)`, `san_pham_id (PK, FK)`, `so_luong_mua`                             |

*Giải thích vì sao thiết kế đạt 3NF:*

Thiết kế ERD này đạt chuẩn 3NF (Third Normal Form) vì tuân thủ các quy tắc sau:

1.  **Đạt 1NF (First Normal Form)**: Tất cả các bảng đều có khóa chính (Primary Key - PK). Không có các thuộc tính đa trị (multi-valued attributes) hay thuộc tính phức hợp (composite attributes). Mỗi cột chỉ chứa một giá trị nguyên tố (atomic value).
2.  **Đạt 2NF (Second Normal Form)**: Mọi thuộc tính không phải là khóa chính (non-key attributes) trong mỗi bảng đều phụ thuộc hoàn toàn vào toàn bộ khóa chính của bảng đó. Ví dụ, trong bảng `Chi_tiet_Don_hang`, thuộc tính `so_luong_mua` phụ thuộc vào cả `don_hang_id` và `san_pham_id` (là khóa chính kết hợp), chứ không phụ thuộc vào chỉ một phần của khóa.
3.  **Đạt 3NF (Third Normal Form)**: Không có sự phụ thuộc bắc cầu (transitive dependency) của các thuộc tính không phải khóa. Tức là, không có thuộc tính không khóa nào phụ thuộc vào một thuộc tính không khóa khác mà không thông qua khóa chính. Ví dụ, trong bảng `Don_hang`, `ma_giam_gia` phụ thuộc trực tiếp vào `id` của `Don_hang`, không phải phụ thuộc vào `khach_hang_id` thông qua một thuộc tính không khóa khác. Tất cả thông tin đều liên quan trực tiếp đến khóa chính của bảng chứa nó.

[Link tới sơ đồ ERD Diagram](erd_diagram.drawio)

### Bước 9 — Vị trí trong tài liệu SRS

Nếu đóng gói các sơ đồ ở Bước 3–8 vào tài liệu SRS theo cấu trúc IEEE 830, các sơ đồ sẽ được đặt tại các vị trí sau:

| Sơ đồ           | Vị trí trong SRS (theo cấu trúc IEEE 830)                                    |
| :-------------- | :---------------------------------------------------------------------------- |
| Use Case Diagram| 3. Chức năng hệ thống (Specific Requirements) -> 3.1. Các Use Case           |
| UI Wireframe    | 3. Chức năng hệ thống (Specific Requirements) -> 3.x. Thiết kế giao diện người dùng (hoặc Phụ lục A. Giao diện người dùng) |
| ERD             | 4. Thiết kế hệ thống (Design) -> 4.1. Cấu trúc dữ liệu                         |
| Class Diagram   | 4. Thiết kế hệ thống (Design) -> 4.1. Cấu trúc dữ liệu                         |

*Giải thích:* Việc đặt các sơ đồ vào đúng vị trí trong SRS giúp tài liệu có cấu trúc logic, dễ theo dõi và tham chiếu. Use Case Diagram mô tả chức năng tổng thể, UI Wireframe minh họa giao diện, còn Class Diagram và ERD cung cấp cái nhìn chi tiết về cấu trúc hệ thống và dữ liệu.