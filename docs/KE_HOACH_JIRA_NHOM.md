# Kế hoạch Jira và phân công nhóm Website Nội Thất

## 1. Nguồn và mục đích

- Nguồn kế hoạch: [Google Sheets Sprint 1 đến Sprint 5](https://docs.google.com/spreadsheets/d/11PtoyuheVmBS-t6Sk2Po8lE-mx_eQj7Vvyp8of9TWxc/edit)
- Thành viên:
  - Trần Đức Lương: nhóm trưởng, backend nghiệp vụ, tích hợp, kiểm thử và bàn giao.
  - Nguyễn Như Kiên: backend nền tảng, xác thực, dữ liệu, kiến trúc và kiểm thử.
  - Phan Thế Kiệt: giao diện khách hàng, giao diện quản trị, UAT và slide.
- Mục đích của file này là giữ một nguồn phân công duy nhất để biết mỗi người cần code gì, ở đâu và nên commit theo thứ tự nào.

> Lưu ý Sprint 1: trong Google Sheets, nội dung các dòng chi tiết đã bị thay bằng chữ `Done`. Vì vậy Sprint 1 được xem là giai đoạn tài liệu đã hoàn thành; tên công việc chi tiết không được suy đoán lại.

## 2. Quy tắc Git của nhóm

- Nhánh cá nhân: `Duc_Luong`, `Nhu_Kien`, `Phan_Kiet`.
- Mỗi Jira sub-task nên tương ứng với ít nhất một commit riêng.
- Không dồn nhiều chức năng không liên quan vào một commit.
- Không commit `.env`, mật khẩu, database thật, thư mục `vendor` hoặc file tạm.
- Trước khi làm: checkout đúng nhánh và pull mới nhất.
- Trước khi commit: chạy kiểm thử liên quan, xem `git status`, chỉ add đúng file của task.
- Chỉ merge vào nhánh tích hợp sau khi task chạy được và không phá test cũ.

### Trạng thái nền tảng hiện tại

- Laravel 11, Docker Compose và GitHub Actions đã được khởi tạo ở commit `084530e`.
- Bốn nhánh `main`, `Duc_Luong`, `Nhu_Kien`, `Phan_Kiet` đang cùng bắt đầu từ commit này.
- Đây là commit nền tảng chung, không tính là chức năng riêng của một thành viên.
- Từ commit tiếp theo, mỗi người chỉ làm trên nhánh cá nhân và chia commit theo Jira task.

Mẫu commit:

```text
feat(products): thêm CRUD và upload ảnh
feat(cart): thêm cập nhật số lượng và kiểm tra tồn kho
test(order): kiểm thử transaction và rollback tồn kho
docs(report): cập nhật hướng dẫn sử dụng
```

## 3. Sprint 1 Project Proposal và phân tích hệ thống

Thời gian trên bảng: 23/09/2026 đến 06/10/2026. Trạng thái hiển thị trên bảng là `Done`.

Các dòng Sprint 1 gồm cả task cha và task con nên không cộng story point theo tên người để tránh tính trùng. Nội dung công việc chi tiết hiện không còn đọc được từ bảng.

Không reset hoặc viết lại các tài liệu Sprint 1 nếu chưa có bản mô tả Jira gốc.

## 4. Sprint 2 Thiết kế dữ liệu kiến trúc và giao diện

### Trần Đức Lương

#### Task 1.1 Thiết kế cơ sở dữ liệu và ERD

- Xác định bảng, thuộc tính, khóa chính, khóa ngoại và quan hệ dữ liệu.
- Bảng chính: users, customer_addresses, categories, products, product_images, carts, orders, order_items, reviews.
- Quy định bảng cần khôi phục dùng `deleted_at`; orders và order_items không xóa, dùng trạng thái.
- File đầu ra: migration trong `database/migrations/`, SQL trong `database/sql/`, ERD trong `docs/`.
- Commit gợi ý:
  - `docs(database): xác định bảng và quan hệ dữ liệu`
  - `feat(database): thêm migration và xóa mềm`
  - `feat(database): thêm bảng đánh giá sau mua hàng`

#### Task 1.3 Thiết kế API phân quyền và nghiệp vụ

- Định nghĩa endpoint, request, response và mã lỗi.
- Xác định API sản phẩm, danh mục, giỏ hàng, đơn hàng và quản trị.
- File dự kiến: `routes/api.php`, `app/Http/Controllers/Api/`, `app/Http/Resources/`, tài liệu API trong `docs/`.
- Commit gợi ý: `docs(api): định nghĩa endpoint và mã lỗi`.

### Nguyễn Như Kiên

- Hoàn thiện ràng buộc, data dictionary và ERD.
- Thiết kế kiến trúc 3 lớp, module và cấu trúc package.
- Vẽ Class Diagram và kiểm tra quan hệ domain.
- Định nghĩa authentication, role, validation và transaction.
- File dự kiến: `docs/`, `app/Services/`, `app/Repositories/`, tài liệu kiến trúc.

### Phan Thế Kiệt

- Wireframe trang khách hàng, sản phẩm, giỏ hàng và checkout.
- Wireframe admin, màu sắc và component dùng chung.
- File dự kiến: `docs/wireframes/` hoặc tài liệu thiết kế giao diện.

## 5. Sprint 3 Nền tảng xác thực và quản lý sản phẩm

### Trần Đức Lương

#### Task 1.1 Khởi tạo Laravel Docker và CI

- Tạo hoặc chuẩn hóa Laravel 11.
- Cấu hình `.env.example`, `Dockerfile`, `docker-compose.yml`.
- Dịch vụ Docker: app, MySQL 8 và phpMyAdmin.
- Trang kiểm tra: Laravel chạy ở cổng 8000, phpMyAdmin ở cổng 8080.
- Commit gợi ý:
  - `chore(project): khởi tạo Laravel 11`
  - `chore(docker): cấu hình Laravel MySQL và phpMyAdmin`

#### Task 1.4 CRUD sản phẩm và tải ảnh

- Backend cần code:
  - Migration và model Product, ProductImage.
  - ProductRepository và ProductService.
  - Admin ProductController và Form Request validation.
  - Upload ảnh vào storage, kiểm tra định dạng và dung lượng.
- Trang liên quan:
  - Danh sách sản phẩm quản trị.
  - Tạo sản phẩm.
  - Sửa sản phẩm.
  - Xem hoặc xóa mềm sản phẩm.
- File dự kiến: `app/Models/Product.php`, `app/Repositories/ProductRepository.php`, `app/Services/ProductService.php`, `app/Http/Controllers/Web/Admin/ProductController.php`, `app/Http/Requests/`, `database/migrations/`.
- Commit gợi ý:
  - `feat(products): thêm model repository và service`
  - `feat(products): thêm CRUD và upload ảnh`

### Nguyễn Như Kiên

- Tạo cấu trúc project, GitHub Actions và seed dữ liệu cơ bản.
- Code đăng ký, đăng nhập, đăng xuất và validation.
- Code middleware admin, authorization và test quyền.
- CRUD danh mục: migration, model, repository, service và controller.
- Tìm kiếm, lọc, phân trang sản phẩm và feature test.
- Trang/backend liên quan: auth, admin categories, ProductController index.

### Phan Thế Kiệt

- Giao diện admin danh mục và validation phía client.
- Giao diện admin sản phẩm, form và thông báo.
- Trang chủ, danh sách sản phẩm và chi tiết sản phẩm.
- File dự kiến: `resources/views/admin/categories/`, `resources/views/admin/products/`, `resources/views/home.blade.php`, `resources/views/products/`.

## 6. Sprint 4 Giỏ hàng đặt hàng và quản lý đơn

### Trần Đức Lương

#### Task 1.1 Xây dựng giỏ hàng

- Backend thêm sản phẩm, sửa số lượng, xóa và chọn sản phẩm.
- Kiểm tra quyền sở hữu dòng giỏ hàng.
- Không cho số lượng vượt tồn kho hoặc nhỏ hơn 1.
- File dự kiến: `CartController`, `CartService`, `CartRepository`, routes và migration carts.
- Commit gợi ý:
  - `feat(cart): thêm sản phẩm và cập nhật số lượng`
  - `feat(cart): kiểm tra tồn kho và quyền sở hữu`

#### Task 1.2 Checkout và đặt hàng COD

- Chỉ đặt các dòng giỏ được chọn.
- Dùng DB transaction và `lockForUpdate` cho từng sản phẩm.
- Kiểm tra tồn kho, tạo order và order_items, trừ kho; lỗi thì rollback.
- Xóa khỏi giỏ chỉ các sản phẩm đã đặt thành công.
- File dự kiến: `OrderController`, `OrderService`, `OrderRepository`, `OutOfStockException`.
- Commit gợi ý:
  - `feat(order): tạo đơn COD trong transaction`
  - `fix(order): khóa tồn kho chống đặt vượt số lượng`

#### Task 1.4 Quản lý đơn phía admin

- Danh sách và chi tiết đơn.
- Chuyển trạng thái hợp lệ: pending -> confirmed -> shipping -> completed.
- Cho phép hủy từ trạng thái hợp lệ; hoàn kho đúng một lần.
- Khi completed, ghi `completed_at`; đây là điều kiện để khách được đánh giá.
- File dự kiến: `AdminOrderController`, `OrderService::updateStatus`, routes admin.
- Commit gợi ý: `feat(admin-orders): quản lý vòng đời đơn hàng`.

### Nguyễn Như Kiên

- Query lịch sử và chi tiết đơn hàng.
- Kiểm tra đơn thuộc khách đang đăng nhập.
- Feature test giỏ hàng, checkout, tồn kho và trạng thái đơn.
- File dự kiến: repository truy vấn đơn, policy hoặc kiểm tra quyền, `tests/Feature/`.

### Phan Thế Kiệt

- Giao diện giỏ hàng, checkbox chọn sản phẩm và tính tổng.
- Giao diện checkout COD và thông báo kết quả.
- Giao diện lịch sử và chi tiết đơn hàng.
- Giao diện admin xử lý, giao, hoàn thành và hủy đơn.
- File dự kiến: `resources/views/cart/`, `resources/views/orders/`, `resources/views/admin/orders/`.

## 7. Sprint 5 Hoàn thiện kiểm thử triển khai và bàn giao

### Trần Đức Lương

#### Test đặt hàng transaction tồn kho và CI

- Kiểm thử đặt đơn thành công.
- Kiểm thử thiếu kho và rollback.
- Kiểm thử hai yêu cầu cạnh tranh không làm âm kho.
- Kiểm thử hủy đơn chỉ hoàn kho một lần.
- Kiểm thử chỉ sản phẩm thuộc đơn completed mới được đánh giá.
- File dự kiến: `tests/Feature/OrderFlowTest.php`, `tests/Feature/ReviewFlowTest.php`, `.github/workflows/ci.yml`.
- Commit gợi ý: `test(order): kiểm thử transaction tồn kho và đánh giá`.

#### Báo cáo hướng dẫn và bàn giao

- Báo cáo kết quả, kiến trúc, kiểm thử và hướng dẫn sử dụng.
- Deploy, cấu hình môi trường và smoke test website.
- Tổng hợp nhánh, xử lý conflict, chạy toàn bộ test rồi tạo PR vào main.
- File dự kiến: `README.md`, `HANDOFF.md`, `docs/`, cấu hình deploy.
- Commit gợi ý:
  - `docs(project): hoàn thiện báo cáo và hướng dẫn`
  - `chore(deploy): cấu hình môi trường triển khai`

### Nguyễn Như Kiên

- Truy vấn dashboard: doanh thu, số đơn, sản phẩm và người dùng.
- Unit và feature test authentication, CRUD và phân quyền.
- File dự kiến: DashboardService hoặc repository thống kê, admin controller, tests.

### Phan Thế Kiệt

- Giao diện dashboard và khóa hoặc mở người dùng.
- Manual test luồng khách, admin, responsive và khả dụng.
- Slide thuyết trình, kịch bản và dữ liệu demo.
- File dự kiến: `resources/views/admin/dashboard*`, `resources/views/admin/users/`, tài liệu slide.

## 8. Trình tự tích hợp đề xuất

1. Sprint 2: hoàn thành thiết kế và chốt schema trước khi code.
2. Sprint 3: Lương dựng Docker; Kiên làm nền tảng/auth/category; Lương làm backend sản phẩm; Kiệt làm giao diện.
3. Sprint 4: Lương làm cart/order/admin order; Kiên làm query và test; Kiệt làm các view tương ứng.
4. Sprint 5: Kiên làm dashboard và test nền tảng; Kiệt hoàn thiện UI/UAT; Lương test transaction, tài liệu, deploy và merge.
5. Chỉ merge khi migration chạy được, test xanh và người phụ trách hiểu phần code của mình.

## 9. Cách hỏi trợ lý ở các phiên sau

Ví dụ:

- `Nhiệm vụ hiện tại của Trần Đức Lương trong Sprint 3 là gì?`
- `Hướng dẫn tôi code Task 1.4 CRUD sản phẩm theo từng commit.`
- `Nguyễn Như Kiên cần làm file nào trong Sprint 4?`
- `Phan Thế Kiệt cần code những trang nào cho checkout?`
- `Kiểm tra code hiện tại đã đủ điều kiện hoàn thành Jira Task 1.2 Sprint 4 chưa.`

Khi trả lời, phải đối chiếu file này và trạng thái Git hiện tại, không tự gán lại nhiệm vụ giữa các thành viên.
