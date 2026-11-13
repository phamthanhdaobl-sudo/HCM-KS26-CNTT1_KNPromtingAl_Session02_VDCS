# HCM-KS26-CNTT1_KNPromtingAl_Session02_VDCS

[Vận dụng chuyên sâu] Thiết kế giải pháp Prompting ứng dụng thực tế cho Cấu trúc Prompt Tiêu chuẩn & Định dạng

Họ và tên: Phạm Thành Đạo
Lớp: HCM-KS26-CNTT1

⸻

I. PHÂN TÍCH BÀI TOÁN VÀ THIẾT KẾ GIẢI PHÁP (I/O)

1. Bối cảnh thực tế

Một cửa hàng kinh doanh trái cây sấy trực tuyến thường xuyên nhận được nhiều đơn hàng từ khách hàng thông qua các nền tảng như Facebook, Zalo và TikTok Shop.

Thông tin đơn hàng được gửi dưới nhiều hình thức khác nhau, có thể thiếu số điện thoại, sai định dạng địa chỉ, ghi nhầm số lượng hoặc không rõ tên sản phẩm.

Nếu nhân viên phải kiểm tra và nhập lại từng đơn hàng bằng phương pháp thủ công sẽ mất nhiều thời gian và dễ xảy ra sai sót.

Vì vậy, em đề xuất xây dựng giải pháp sử dụng AI để tự động tiếp nhận, phân tích, kiểm tra và chuẩn hóa thông tin đơn hàng.

2. Mục tiêu giải pháp

* Tự động trích xuất thông tin từ nội dung đơn hàng.
* Chuẩn hóa tên sản phẩm, số lượng và thông tin khách hàng.
* Phát hiện những trường dữ liệu bị thiếu hoặc không hợp lệ.
* Phân loại trạng thái đơn hàng.
* Xuất kết quả theo định dạng JSON để có thể sử dụng trong các hệ thống quản lý dữ liệu.
* Xuất thêm bảng tổng hợp giúp nhân viên dễ dàng theo dõi.

3. Thiết kế Input

Dữ liệu đầu vào là nội dung đơn hàng do khách hàng gửi đến cửa hàng.

Các trường thông tin cần xử lý:

Trường dữ liệu	Ý nghĩa
Mã đơn hàng	Mã định danh đơn hàng
Họ tên	Tên người nhận
Số điện thoại	Số điện thoại liên hệ
Địa chỉ	Địa chỉ giao hàng
Sản phẩm	Tên sản phẩm khách đặt
Số lượng	Số lượng sản phẩm
Đơn giá	Giá tiền của một sản phẩm
Ghi chú	Yêu cầu đặc biệt của khách hàng

Ví dụ dữ liệu đầu vào:

Mã đơn: DH001
Khách hàng: Nguyễn Văn An
SĐT: 0987654321
Địa chỉ: 25 Nguyễn Trãi, Quận 5, TP.HCM
Sản phẩm: Mít sấy
Số lượng: 2 gói
Đơn giá: 45000 đồng/gói
Ghi chú: Giao giờ hành chính.

4. Thiết kế Output

Đầu ra cần có hai dạng:

a. JSON: Dùng để lưu trữ hoặc tích hợp vào hệ thống quản lý đơn hàng.

b. Bảng tổng hợp: Dùng để nhân viên cửa hàng dễ dàng kiểm tra và theo dõi.

Các trạng thái đơn hàng:

* HỢP_LỆ: Đầy đủ thông tin bắt buộc và dữ liệu hợp lệ.
* THIẾU_THÔNG_TIN: Có trường dữ liệu bắt buộc bị thiếu.
* CẦN_XÁC_MINH: Có dữ liệu sai định dạng, mâu thuẫn hoặc chưa rõ ràng.

II. THIẾT KẾ PROMPT VÀ ÁP DỤNG KỸ THUẬT

1. Cấu trúc Prompt tiêu chuẩn (CREATE)

Prompt được xây dựng theo các thành phần:

* C – Context: Cung cấp bối cảnh cửa hàng kinh doanh trái cây sấy.
* R – Role: Giao vai trò chuyên viên xử lý dữ liệu đơn hàng.
* E – Explicit Task: Xác định nhiệm vụ trích xuất và kiểm tra dữ liệu.
* A – Audience: Xác định người sử dụng kết quả là nhân viên cửa hàng.
* T – Tone: Yêu cầu phản hồi rõ ràng, chính xác, chuyên nghiệp.
* E – Expected Format: Ép AI trả kết quả theo cấu trúc JSON và bảng Markdown.

2. Kỹ thuật Few-shot Prompting

Cung cấp cho AI các ví dụ mẫu về cách xử lý đơn hàng để AI hiểu quy tắc phân loại.

Ví dụ 1 – Đơn hàng hợp lệ:

Input: Khách hàng Nguyễn Văn An, SĐT 0987654321, địa chỉ 25 Nguyễn Trãi, TP.HCM, đặt 2 gói mít sấy, đơn giá 45.000 đồng.

Output: Trạng thái HỢP_LỆ, tổng tiền 90.000 đồng.

Ví dụ 2 – Đơn hàng thiếu thông tin:

Input: Khách hàng Trần Minh, đặt 3 gói chuối sấy, đơn giá 35.000 đồng, không có số điện thoại và địa chỉ.

Output: Trạng thái THIẾU_THÔNG_TIN, các trường cần bổ sung là số điện thoại và địa chỉ.

Ví dụ 3 – Đơn hàng cần xác minh:

Input: Khách hàng Lê Văn B, SĐT 12345, đặt 2 gói mít sấy.

Output: Trạng thái CẦN_XÁC_MINH, lý do số điện thoại không đúng định dạng thông thường.

Thông qua các ví dụ trên, AI có thể học cách phân biệt và xử lý các trường hợp dữ liệu khác nhau.

3. Kỹ thuật ép định dạng đầu ra

Sử dụng yêu cầu định dạng JSON để đảm bảo dữ liệu có cấu trúc thống nhất.

Các nguyên tắc:

* Chỉ sử dụng đúng các trường dữ liệu đã quy định.
* Không tự ý tạo thêm thông tin khách hàng.
* Trường dữ liệu không có thông tin phải trả về null.
* Giá trị tiền tệ phải là số.
* JSON phải đúng cú pháp, không thêm lời giải thích bên ngoài JSON khi được yêu cầu xuất JSON thuần.
* Bảng Markdown phải thể hiện rõ trạng thái và lý do xử lý.

III. PROMPT THỰC THI HOÀN CHỈNH

C – CONTEXT:
Bạn đang hỗ trợ một cửa hàng kinh doanh trái cây sấy trực tuyến.
Cửa hàng thường xuyên nhận đơn hàng từ nhiều nền tảng khác nhau.
Thông tin khách hàng có thể thiếu, sai định dạng hoặc không thống nhất.
R – ROLE:
Bạn là chuyên viên xử lý và chuẩn hóa dữ liệu đơn hàng,
có nhiệm vụ kiểm tra tính đầy đủ và chính xác của thông tin.
E – EXPLICIT TASK:
Thực hiện các nhiệm vụ sau:
1. Trích xuất thông tin đơn hàng.
2. Chuẩn hóa tên sản phẩm và đơn vị tính.
3. Kiểm tra các trường thông tin bắt buộc:
   - Mã đơn hàng.
   - Họ tên khách hàng.
   - Số điện thoại.
   - Địa chỉ giao hàng.
   - Tên sản phẩm.
   - Số lượng.
   - Đơn giá.
4. Tính tổng tiền = số lượng × đơn giá.
5. Phân loại trạng thái đơn hàng.
QUY TẮC XỬ LÝ LỖI:
- Nếu thiếu thông tin bắt buộc: THIẾU_THÔNG_TIN.
- Nếu số điện thoại sai định dạng, số lượng không hợp lệ,
  đơn giá không hợp lệ hoặc dữ liệu mâu thuẫn:
  CẦN_XÁC_MINH.
- Nếu đầy đủ và hợp lệ: HỢP_LỆ.
- Nếu dữ liệu thiếu, không được tự suy đoán hoặc tự tạo thông tin.
- Nếu không xác định được giá trị, trả về null.
- Nếu số lượng hoặc đơn giá không hợp lệ, không tính tổng tiền,
  trả về null.
- Không tự ý sửa dữ liệu sai lệch khi chưa có căn cứ.
- Nếu có nhiều lỗi, phải liệt kê đầy đủ.
- Không tự xác nhận đơn hàng đã được thanh toán hoặc giao thành công.
FEW-SHOT EXAMPLES:
Ví dụ 1:
Input:
Mã đơn: DH001
Tên: Nguyễn Văn An
SĐT: 0987654321
Địa chỉ: 25 Nguyễn Trãi, TP.HCM
Sản phẩm: Mít sấy
Số lượng: 2
Đơn giá: 45000
Output:
Trạng thái: HỢP_LỆ
Tổng tiền: 90000
Ví dụ 2:
Input:
Mã đơn: DH002
Tên: Trần Minh
Sản phẩm: Chuối sấy
Số lượng: 3
Đơn giá: 35000
Output:
Trạng thái: THIẾU_THÔNG_TIN
Lỗi: Thiếu số điện thoại và địa chỉ.
Ví dụ 3:
Input:
Mã đơn: DH003
Tên: Lê Văn B
SĐT: 12345
Địa chỉ: 10 Lê Lợi, TP.HCM
Sản phẩm: Mít sấy
Số lượng: 2
Đơn giá: 45000
Output:
Trạng thái: CẦN_XÁC_MINH
Lỗi: Số điện thoại không đúng định dạng thông thường.
A – AUDIENCE:
Kết quả được sử dụng bởi nhân viên quản lý đơn hàng.
Ưu tiên thông tin rõ ràng, dễ kiểm tra và có thể sử dụng ngay.
T – TONE:
Chuyên nghiệp, chính xác, ngắn gọn, không suy diễn.
E – EXPECTED FORMAT:
Hãy trả về đúng hai phần:
PHẦN 1: JSON
Sử dụng cấu trúc:
{
  "ma_don": "",
  "ho_ten": "",
  "so_dien_thoai": "",
  "dia_chi": "",
  "san_pham": "",
  "so_luong": null,
  "don_gia": null,
  "tong_tien": null,
  "trang_thai": "",
  "loi_phat_hien": []
}
PHẦN 2: BẢNG MARKDOWN
| Mã đơn | Khách hàng | Sản phẩm | Số lượng |
| Đơn giá | Tổng tiền | Trạng thái | Lỗi |
Yêu cầu:
- JSON phải đúng cú pháp.
- Không thêm thông tin ngoài dữ liệu đầu vào.
- Sử dụng null cho trường không xác định.
- Không bỏ sót lỗi dữ liệu.
DỮ LIỆU ĐẦU VÀO CẦN XỬ LÝ:
Mã đơn: DH004
Khách hàng: Phạm Minh Tuấn
SĐT: 0901234567
Địa chỉ: 15 Nguyễn Văn Cừ, Quận 5, TP.HCM
Sản phẩm: Mít sấy
Số lượng: 3
Đơn giá: 45000
Ghi chú: Giao hàng giờ hành chính.

IV. KẾT QUẢ CHẠY THỬ PROMPT

Sau khi đưa prompt vào AI, kết quả phản hồi dự kiến như sau:

1. Kết quả JSON

{
  "ma_don": "DH004",
  "ho_ten": "Phạm Minh Tuấn",
  "so_dien_thoai": "0901234567",
  "dia_chi": "15 Nguyễn Văn Cừ, Quận 5, TP.HCM",
  "san_pham": "Mít sấy",
  "so_luong": 3,
  "don_gia": 45000,
  "tong_tien": 135000,
  "trang_thai": "HỢP_LỆ",
  "loi_phat_hien": []
}

2. Kết quả bảng Markdown

Mã đơn	Khách hàng	Sản phẩm	Số lượng	Đơn giá	Tổng tiền	Trạng thái
DH004	Phạm Minh Tuấn	Mít sấy	3	45.000đ	135.000đ	HỢP_LỆ

Ghi chú: Đơn hàng có đầy đủ thông tin bắt buộc, số điện thoại đúng định dạng thông thường và phép tính tổng tiền chính xác.

V. KIỂM THỬ CÁC TRƯỜNG HỢP NGOẠI LỆ

Để đánh giá độ tin cậy của giải pháp, em tiến hành kiểm tra thêm các trường hợp dữ liệu bất thường.

Trường hợp	Dữ liệu lỗi	Kết quả mong muốn
Thiếu SĐT	Không nhập số điện thoại	THIẾU_THÔNG_TIN
Sai SĐT	SĐT chỉ có 5 chữ số	CẦN_XÁC_MINH
Thiếu địa chỉ	Không có địa chỉ giao hàng	THIẾU_THÔNG_TIN
Số lượng âm	Số lượng = -2	CẦN_XÁC_MINH
Thiếu đơn giá	Không cung cấp giá bán	THIẾU_THÔNG_TIN
Dữ liệu đầy đủ	Các trường hợp lệ	HỢP_LỆ

Lưu ý: Các trường hợp trên là kết quả mong muốn khi kiểm thử quy tắc, không phải kết quả thực nghiệm độc lập.

VI. ĐÁNH GIÁ VÀ KẾT LUẬN

1. Ưu điểm

* Giảm thời gian nhập liệu và kiểm tra đơn hàng thủ công.
* Hạn chế sai sót khi xử lý dữ liệu.
* Chuẩn hóa thông tin theo một cấu trúc thống nhất.
* Dễ dàng tích hợp với hệ thống quản lý đơn hàng.
* Có thể mở rộng để xử lý số lượng lớn đơn hàng.

2. Hạn chế

* AI vẫn có thể nhận diện sai thông tin nếu dữ liệu đầu vào không rõ ràng.
* Cần quy định cụ thể về định dạng số điện thoại, sản phẩm và đơn giá.
* Kết quả AI cần được nhân viên kiểm tra trước khi xác nhận đơn hàng.
* JSON đúng cú pháp chưa đồng nghĩa với dữ liệu hoàn toàn chính xác.

3. Kết luận

Qua bài tập này, em đã vận dụng được cấu trúc Prompt tiêu chuẩn CREATE, kỹ thuật Few-shot Prompting và phương pháp ép định dạng đầu ra JSON/Table để xây dựng một giải pháp xử lý đơn hàng thực tế.

Việc kết hợp các kỹ thuật này giúp AI hiểu rõ vai trò, nhiệm vụ, quy tắc xử lý và định dạng kết quả cần trả về. Đặc biệt, việc thiết kế các điều kiện xử lý dữ liệu thiếu hoặc sai lệch giúp hạn chế tình trạng AI tự suy đoán thông tin.

Đây là nền tảng quan trọng để phát triển các giải pháp tự động hóa công việc bằng AI trong môi trường doanh nghiệp.

⸻

HẾT
