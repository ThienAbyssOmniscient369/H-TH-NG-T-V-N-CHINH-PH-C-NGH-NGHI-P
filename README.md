# H-TH-NG-T-V-N-CHINH-PH-C-NGH-NGHI-P
Một trang web hỗ trợ - hướng dẫn tra cứu thông tin nghành nghề, trường và một số tiện ích khác

# Hệ thống Tư vấn và Định hướng Tuyển sinh

Website hỗ trợ học sinh lớp 12 ghi nhận mục tiêu học tập, tham khảo thông tin tuyển sinh, xây dựng kế hoạch ôn tập và theo dõi thời gian còn lại đến các kỳ thi.

## Giới thiệu

Dự án là một ứng dụng web tĩnh gồm hai trang HTML. Trang bìa tiếp nhận tên người dùng và email, sau đó chuyển sang trang tư vấn. Trong trang tư vấn, người dùng có thể lập kế hoạch theo trường mục tiêu, xem bảng điểm tham khảo, quy đổi một số tổ hợp điểm và lưu kế hoạch vào hồ sơ cá nhân.

Ứng dụng chạy phía trình duyệt, không cần máy chủ ứng dụng hay cơ sở dữ liệu. Hồ sơ và thông tin đăng nhập được lưu trong bộ nhớ trình duyệt của người dùng.

## Thành phần hệ thống

| Tệp | Vai trò | Chức năng chính |
|---|---|---|
| [`html/coverpage_updated.html`](html/coverpage_updated.html) | Trang bìa và đăng nhập | Thu thập tên, email; lưu thông tin nhận diện; mở trang tư vấn. |
| [`html/web tư vấn tuyển sinh.html`](<html/web tư vấn tuyển sinh.html>) | Ứng dụng tư vấn chính | Chứa các tab Trang chủ, Chiến lược, Điểm chuẩn, Tính điểm, Cẩm nang và Hồ sơ cá nhân. |
| [`images/coverpage-image.png`](images/coverpage-image.png) | Tài nguyên hình ảnh | Ảnh hội chợ hướng nghiệp hiển thị trên trang bìa. |

### Kiến trúc

- **Giao diện:** HTML, CSS, SVG nội tuyến và phông chữ Plus Jakarta Sans. CSS điều chỉnh bố cục cho màn hình nhỏ và lớn.
- **Điều hướng:** thanh tab ngang dùng chung; JavaScript chuyển nội dung bằng cách bật/tắt trạng thái tab.
- **Nghiệp vụ:** JavaScript trong trang tư vấn xử lý tạo kế hoạch, đếm ngược, lọc điểm tham khảo và quy đổi điểm.
- **Lưu trữ:** `localStorage` lưu hồ sơ theo tên người dùng; `sessionStorage` là phương án dự phòng. Thông tin đăng nhập được ghi vào hai vùng lưu trữ này để trang tư vấn nhận diện người dùng.
- **Máy chủ:** không có backend, API hay cơ sở dữ liệu phía máy chủ trong phiên bản hiện tại.
- **Tài nguyên ngoài:** giao diện tải Plus Jakarta Sans từ Google Fonts; một số ảnh minh họa trên trang chủ dùng dịch vụ ảnh placeholder. Ảnh chính của trang bìa nằm trong thư mục `images`.

## Nguyên lý hoạt động

1. Mở trang bìa, nhập tên người dùng và email hợp lệ, rồi gửi biểu mẫu.
2. Trang bìa lưu thông tin nhận diện vào bộ nhớ trình duyệt và điều hướng đến trang tư vấn cùng thư mục.
3. Người dùng mở tab **Chiến lược**, nhập ngày khảo sát, ngày thi, mục tiêu điểm và trường mong muốn.
4. Khi nhấn nút tạo chiến lược, ứng dụng dựng kết quả tư vấn và lưu một bản ghi hồ sơ. Hồ sơ chỉ xuất hiện sau lần tạo chiến lược thành công đầu tiên.
5. Tab **Hồ sơ cá nhân** đọc bản ghi đã lưu, hiển thị các mục tiêu và cập nhật đồng hồ đếm ngược. IELTS, SAT và TOEIC hiện được lưu dưới dạng mục tiêu điểm; biểu mẫu chưa có trường ngày thi riêng cho các chứng chỉ này.
6. Các tab còn lại hoạt động độc lập trong cùng trang: bảng điểm lọc dữ liệu tĩnh theo trường/ngành, công cụ tính điểm xử lý các giá trị người dùng nhập, và Cẩm nang hiển thị nội dung hướng dẫn.

## Thuật toán và xử lý dữ liệu

### Tạo kế hoạch và xác định ngày thi

- Ngày khảo sát mặc định là ngày hiện tại theo giờ địa phương.
- Nếu người dùng chưa chỉnh ngày thi, hệ thống đặt ngày dự kiến V-ACT vào **30/03** và THPTQG vào **26/06**. Năm mặc định là năm hiện tại hoặc năm kế tiếp tùy thời điểm trong năm.
- Khi ngày khảo sát đã sau ngày thi được chọn, thuật toán chuyển ngày thi sang năm kế tiếp.
- Người dùng cần nhập trường mục tiêu để tạo kế hoạch. Tên trường được dùng để chọn nội dung gợi ý: tên có từ khóa như “quốc tế”, “OISP”, “international”, “IU” hoặc “tiên tiến” sẽ hiển thị nhóm gợi ý cho chương trình quốc tế/tiên tiến; các trường hợp khác dùng lộ trình chung.
- Kế hoạch lưu ngày khảo sát, trường mục tiêu, ngày V-ACT, ngày THPTQG, các mục tiêu THPTQG/V-ACT/IELTS/SAT/TOEIC và thời điểm tạo.

### Vòng đếm ngược

Với ngày thi `T`, ngày khảo sát `S` và thời điểm hiện tại `N`, giao diện cập nhật số ngày cùng đồng hồ giờ, phút, giây mỗi giây. Phần vòng cung gradient biểu thị phần thời gian còn lại:

```text
thời_lượng = max(1, T - S)
tỷ_lệ_còn_lại = clamp((T - N) / thời_lượng, 0, 1)
độ_dịch_vòng = chu_vi × (1 - tỷ_lệ_còn_lại)
```

Khi đến ngày thi, số ngày và đồng hồ về 0, vòng cung biến mất và giao diện báo đã đến ngày thi. Ngày thi dùng trong hồ sơ là ngày lịch, không bao gồm giờ thi cụ thể.

Số ngày hiển thị được lấy bằng phần nguyên của thời gian còn lại chia cho 24 giờ; đồng hồ bên dưới hiển thị phần giờ, phút và giây còn lại.

### Quy đổi điểm

Tab **Tính điểm** có các phép tính tham khảo sau:

- IELTS được quy đổi theo các mốc: từ 5.0 → 8.0; 5.5 → 8.5; 6.0 → 9.0; 6.5 → 9.5; từ 7.0 → 10.0.
- Điểm ước tính tổ hợp Bách Khoa được tính theo công thức `(V-ACT / 1200 × 70) + (THPT / 30 × 20) + (GPA / 10 × 10) + điểm thưởng`. Khi bỏ trống GPA, thành phần GPA mặc định là 8 điểm. Điểm thưởng là 5 nếu IELTS từ 6.5 hoặc SAT từ 1300; nếu không, là 3 khi IELTS từ 6.0 hoặc SAT từ 1200; các trường hợp còn lại không cộng điểm.
- Điểm THPT quy đổi cho tổ hợp A01/D01 được tính bằng tổng điểm hai môn đã nhập và điểm tiếng Anh quy đổi từ IELTS, nếu người dùng cung cấp đủ dữ liệu.

Đây là các công thức tham khảo được cài đặt trong trang, không thay thế phương án xét tuyển chính thức của trường.

## Dữ liệu và quyền riêng tư

- `admissionUser` chứa tên, email và thời điểm đăng nhập; hồ sơ kế hoạch có khóa dạng `admissionProfile:<tên-người-dùng>`.
- Dữ liệu nằm trong trình duyệt hiện tại; xóa dữ liệu website/trình duyệt có thể xóa hồ sơ. Dự án chưa đồng bộ hồ sơ giữa các thiết bị.
- Biểu mẫu đăng nhập chỉ nhận diện người dùng trong giao diện. Đây không phải hệ thống xác thực tài khoản có mật khẩu.
- Bảng điểm chuẩn là dữ liệu tĩnh nằm trong mã nguồn và có thể đã cũ. Hãy đối chiếu thông tin tuyển sinh, ngày thi và quy đổi điểm với thông báo chính thức trước khi ra quyết định.

## Chạy dự án

Giữ nguyên cấu trúc thư mục để đường dẫn ảnh và điều hướng giữa hai trang hoạt động. Từ thư mục gốc dự án, có thể chạy máy chủ tĩnh Python:

```bash
python -m http.server 8000
```

Sau đó mở [http://localhost:8000/html/coverpage_updated.html](http://localhost:8000/html/coverpage_updated.html). Có thể dùng tiện ích Live Server trong Visual Studio Code thay thế. Không cần cài thư viện JavaScript hay bước build.

```text
VS code/
├── html/
│   ├── coverpage_updated.html
│   └── web tư vấn tuyển sinh.html
└── images/
    └── coverpage-image.png
```

## Mã nguồn mở

> **Mã nguồn mở! Có thể làm nguồn tư liệu tham khảo miễn phí.
