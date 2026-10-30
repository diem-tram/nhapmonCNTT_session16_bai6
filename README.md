Mô hình lưu trữ Hybrid Workflow (An toàn Code cảm biến)
Đội Dev áp dụng mô hình lưu trữ kết hợp Git/GitHub (Code & Logic) và Cloud Storage - Google Drive Enterprise (Dữ liệu lớn & Config):
Luồng làm việc (Workflow):
Local Dev Environment: Lập trình viên viết code đọc dữ liệu cảm biến (nhiệt độ, độ ẩm, độ pH) trên máy cá nhân.
Git Repository (GitHub): Quản lý phiên bản code, phân nhánh main (production), develop (staging), và feature/* (tính năng mới). Mọi thay đổi đều phải qua Pull Request (PR) và có ít nhất 1 thành viên review.
Bảo mật File .env (Mật khẩu & API Key cảm biến):
Tệp .env chứa mật khẩu kết nối cảm biến, token database tuyệt đối KHÔNG được commit lên Git (đã thêm vào tệp .gitignore).
Tệp .env.example được đưa lên repo để làm mẫu cấu trúc (chỉ chứa các key trống, không chứa mật khẩu thực tế).
Tệp .env thực tế được lưu trữ trên Google Drive ở thư mục bảo mật cao, chỉ Quản trị viên (Admin) và Dev lead có quyền truy cập để bàn giao khi Deploy.
Cấu trúc Folder & Phân quyền Truy cập (Google Drive / Enterprise Cloud)
Cấu trúc cây thư mục:
Plaintext
GreenTech_SmartFarm_Workspace/
├── 01_SourceCode_Sensors/          (Code & Firmware cảm biến)
├── 02_Reports_Investors/           (Báo cáo tài chính & Tiến độ cho Nhà đầu tư)
├── 03_Hardware_Vendors/            (Tài liệu kỹ thuật & Thiết bị từ Đối tác)
└── 04_Operations_Config/           (Tài liệu cấu hình hệ thống & .env backup)
Ma trận phân quyền (Permissions Matrix):
Nhóm người dùng	Thư mục truy cập	Quyền hạn (Role)	Ràng buộc bảo mật
Đội ngũ Dev (Nội bộ)	01_SourceCode_Sensors, 04_Operations_Config	Editor (Người chỉnh sửa)	Được push code, sửa file config, không được xoá thư mục gốc.
Nhà đầu tư (Investors)	02_Reports_Investors	Viewer (Người xem)	Chỉ xem báo cáo tiến độ/tài chính. Tắt tính năng tải xuống, in và sao chép.
Đối tác thiết bị (Vendors)	03_Hardware_Vendors	Commenter (Người nhận xét)	Đọc tài liệu thông số kỹ thuật và để lại nhận xét/góp ý trực tiếp trên file.
Cấu trúc Kênh trao đổi & Quy trình chuyển Thảo luận thành Task (Enterprise Workspace - Slack/Trello/Jira)
Cấu trúc Kênh (Channels):
#announcement: Thông báo chung toàn dự án (Chỉ Admin được đăng bài).
#dev-sensors: Trao đổi kỹ thuật, lập trình cảm biến.
#alert-sensor-bugs: Kênh nhận cảnh báo tự động & thảo luận về lỗi cảm biến.
#vendor-support: Trao đổi với đối tác cung cấp thiết bị.
Quy trình chuyển Thảo luận thành Task có Deadline (Luồng xử lý Bug cảm biến):
Phát hiện lỗi: Cảm biến gửi log lỗi về kênh #alert-sensor-bugs (Ví dụ: "Sensor_DHT22_Zone1 ngắt kết nối").
Thảo luận nhanh: Dev trực ca thảo luận nguyên nhân trong thread của tin nhắn đó.
Tạo Task tự động/Thủ công:
Sử dụng tích hợp (Integration) giữa Slack/Teams và Trello/Jira.
Click vào menu tin nhắn lỗi → Chọn "Create Jira/Trello Task".
Điền thông tin Task tiêu chuẩn:
Title: [BUG] Cảm biến DHT22 Khu vực 1 mất tín hiệu
Assignee: Gán trực tiếp cho Dev phụ trách phần cứng/mạng.
Priority: High / Critical.
Deadline: Rõ ràng (Ví dụ: 17:00 PM cùng ngày).
Link đính kèm: Dẫn trực tiếp link thread thảo luận để giữ nguyên ngữ cảnh.
Quy trình Bàn giao Dự án (Transfer Ownership)
Khi bài tập nhóm hoặc dự án kết thúc, Trưởng dự án thực hiện bàn giao toàn bộ tài sản số:
Chuyển quyền GitHub Repository:
Vào Settings → Danger Zone → Chọn Transfer ownership.
Nhập username/organization của giảng viên hoặc Trưởng nhóm mới → Xác nhận chuyển giao.
Chuyển quyền Google Drive Folder:
Chọn Thư mục gốc dự án → Share → Tìm tài khoản nhận bàn giao → Đổi quyền từ Editor thành Make Owner (Làm chủ sở hữu).
Bàn giao File bí mật .env:
Gửi trực tiếp qua kênh bảo mật cá nhân cho Owner mới, hủy tất cả quyền truy cập cũ của các thành viên đã hết nhiệm vụ.
QUY TRÌNH KHÔI PHỤC DỮ LIỆU KHI BỊ XÓA (EMERGENCY RECOVERY)
Kịch bản sự cố: Một thành viên lỡ tay xóa sạch thư mục 01_SourceCode_Sensors trên Google Drive/Cloud Workspace.
Quy trình khôi phục nhanh nhất (3 bước):
Bước 1: Khai thác Thùng rác hệ thống (Trash/Recycle Bin)
Người quản trị (Admin/Owner) vào mục Trash (Thùng rác) của Google Drive / Shared Drive.
Tìm thư mục 01_SourceCode_Sensors bị xóa (các mục đã xóa thường lưu giữ 30 ngày).
Chuột phải → Chọn Restore (Khôi phục). Thư mục và toàn bộ phân quyền cũ sẽ quay lại vị trí ban đầu trong vài giây.
Bước 2: Khôi phục Code từ Git Remote / Local Repository (Nếu dữ liệu trên Drive hỏng nặng)
Do áp dụng Hybrid Workflow, toàn bộ source code cảm biến luôn được đồng bộ an toàn trên GitHub Repository.
Thành viên Dev chỉ cần mở terminal tại máy local và chạy lệnh:
Bash
git checkout main
git pull origin main
Code chuẩn lập tức được kéo về đầy đủ mà không mất dòng code nào.
Bước 3: Rà soát & Cấu hình lại phân quyền (Prevention)
Kiểm tra lý do sự cố: Do phân quyền cho thành viên quá cao.
Chỉnh sửa quyền của thành viên từ Content Manager / Editor xuống quyền hạn hẹp hơn hoặc bật tính năng Giao diện đĩa dùng chung (Shared Drive - Prevent members from deleting files) để tránh việc xóa nhầm trong tương lai.

