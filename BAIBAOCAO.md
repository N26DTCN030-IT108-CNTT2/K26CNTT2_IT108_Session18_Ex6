# Báo Cáo Tư Vấn Bảo Mật Toàn Diện Cho Lập Trình Viên Tự Do (Freelancer)
**Người thực hiện:** Lê Hồ Thanh Thảo

## 1. Chuẩn đoán Rủi ro Môi trường Mạng Công Cộng
Làm việc tại không gian mở như quán cafe tiềm ẩn 3 nhóm rủi ro an ninh mạng cốt lõi:
* **Nghe lén dữ liệu (Man-in-the-Middle/Packet Sniffing):** Kẻ tấn công chung mạng Wi-Fi có thể đánh cắp cookie phiên đăng nhập, API keys hoặc dữ liệu không được mã hóa.
* **Tấn công thị giác (Shoulder Surfing):** Góc nhìn mở khiến màn hình dễ bị người xung quanh quan sát, làm lộ mật khẩu hoặc mã nguồn dự án.
* **Tiếp cận vật lý trái phép:** Nguy cơ mất cắp thiết bị hoặc bị can thiệp phần cứng (cắm USB chứa mã độc) khi rời khỏi bàn trong thời gian ngắn.

## 2. Thiết lập Két sắt số Bitwarden
* **Xây dựng Master Password chuẩn quân sự:** Khước từ việc sử dụng một từ khóa đơn giản. Thay vào đó, sử dụng **Passphrase** (Cụm từ mật khẩu) kết hợp 4-5 từ ngẫu nhiên, số và ký tự đặc biệt, đảm bảo độ dài trên 16 ký tự (VD: `Ly-C@fe-Khong-Duong-Tren-Ban-So-9!`).
* **Quản lý phân quyền:** Lưu trữ toàn bộ thông tin đăng nhập, token hệ thống vào Bitwarden. Tắt hoàn toàn tính năng tự động lưu mật khẩu mặc định trên trình duyệt để tránh rò rỉ khi trình duyệt bị khai thác lỗ hổng.
* **Cơ chế Auto-lock:** Cấu hình Két sắt số tự động khóa (yêu cầu nhập lại Master Password) ngay khi màn hình máy tính bị khóa hoặc sau 5 phút không có hoạt động.

## 3. Kích hoạt 2FA (TOTP) & Quản lý Backup Codes
* **Cài đặt Authenticator App:** Kích hoạt xác thực 2 bước bằng mã OTP theo thời gian thực (TOTP) thông qua các ứng dụng như Google Authenticator hoặc Microsoft Authenticator trên thiết bị di động cá nhân, tách biệt với máy tính làm việc.
* **Quy trình lưu trữ Backup Codes an toàn:**
  * **Cảnh báo:** Tuyệt đối không chụp ảnh màn hình, không lưu vào Cloud (Google Drive/iCloud), và không lưu trong file `.txt` trên máy tính.
  * **Triển khai:** In toàn bộ Backup Codes ra giấy và cất vào két sắt vật lý, hoặc chép vào một USB mã hóa chuyên dụng không kết nối mạng internet.

## 4. Quy trình đẩy Code lên Git sạch bóng Secrets
* **Sử dụng Biến môi trường (Environment Variables):** Tuyệt đối không hard-code (gắn cứng) API Keys, Access Tokens hay Database Passwords vào mã nguồn. Khai báo chúng vào tệp cục bộ `.env`.
* **Khai báo `.gitignore` chặt chẽ:** Đảm bảo tệp `.gitignore` luôn chứa các cấu hình nhạy cảm (`.env`, `*.pem`, `id_rsa`) trước khi thực hiện lệnh `git add` lần đầu tiên.
* **Tích hợp Pre-commit Hooks tự động:** Cài đặt các công cụ quét mã nguồn như **TruffleHog** hoặc **git-secrets**. Cấu hình này sẽ chặn đứng lệnh `git commit` nếu phát hiện các chuỗi văn bản có định dạng khớp với token, AWS keys hay mật khẩu.

**1. Phân tích bối cảnh & Nhận diện rủi ro (Môi trường quán Cafe)**
Làm việc tại không gian công cộng tiềm ẩn 3 nhóm rủi ro an ninh mạng và an toàn thông tin cốt lõi:

Nghe lén dữ liệu (Man-in-the-Middle/Packet Sniffing): Kẻ gian chung mạng Wi-Fi công cộng có thể đánh cắp cookie phiên đăng nhập, API keys hoặc dữ liệu không mã hóa.

Tấn công thị giác (Shoulder Surfing): Góc nhìn mở khiến màn hình dễ bị người xung quanh hoặc camera an ninh quay lén khi gõ mật khẩu, để lộ mã nguồn hoặc thông tin dự án.

Tiếp cận vật lý trái phép: Nguy cơ mất cắp thiết bị hoặc bị cắm USB chứa mã độc khi rời khỏi bàn trong thời gian ngắn (đi vệ sinh, nghe điện thoại).

**2. Hướng dẫn triển khai các cấu hình bảo mật kỹ thuật**

2.1. Thiết lập Két sắt số Bitwarden với Master Password chuẩn quân sự

Xây dựng Master Password: Không dùng một từ phức tạp, hãy dùng Passphrase (Cụm từ mật khẩu). Cấu trúc đề xuất: kết hợp 4-5 từ ngẫu nhiên, số và ký tự đặc biệt (VD: Ly-C@fe-Khong-Duong-Tren-Ban-So-9!). Đảm bảo độ dài trên 16 ký tự.

Vô hiệu hóa trình quản lý của trình duyệt: Tắt tính năng tự động lưu mật khẩu trên Chrome/Edge/Firefox và xóa toàn bộ mật khẩu cũ đã lưu ở trình duyệt để tránh rò rỉ khi trình duyệt bị khai thác.

Thiết lập Auto-lock (Tự động khóa): Cấu hình extension Bitwarden tự động khóa (yêu cầu nhập lại Master Password hoặc mã PIN) ngay khi khóa màn hình máy tính hoặc sau 5-10 phút không hoạt động.

2.2. Kích hoạt 2FA (TOTP) & Quản lý Backup Codes

Sử dụng Authenticator App: Cài đặt ứng dụng tạo mã OTP theo thời gian thực (TOTP) như Google Authenticator, Microsoft Authenticator hoặc Authy trên điện thoại cá nhân (không lưu mã 2FA trên cùng thiết bị máy tính làm việc để đảm bảo nguyên tắc xác thực 2 yếu tố độc lập).

Quản lý Backup Codes (Mã dự phòng):

Khi bật 2FA cho Bitwarden và Git, hệ thống sẽ cấp một danh sách Backup Codes.

Tuyệt đối không: Chụp màn hình, lưu vào Google Drive/iCloud, hoặc để trong file .txt trên máy.

Cách an toàn: In ra giấy và cất vào két sắt vật lý tại nhà, hoặc chép vào một USB được mã hóa chuyên dụng không kết nối mạng.

2.3. Quy trình đẩy code lên Git sạch bóng Secrets

Quản lý biến môi trường: Tuyệt đối không hard-code (gắn cứng) API Keys, Access Tokens, hay Database Passwords vào mã nguồn. Khai báo chúng vào file .env.

Thiết lập .gitignore chặt chẽ: Đảm bảo file .gitignore luôn chứa các tệp tin cấu hình nhạy cảm và thư mục rác trước khi thực hiện lệnh git add. (VD: .env, *.pem, id_rsa, node_modules/).

Quét Secrets tự động (Pre-commit Hook): Tích hợp các công cụ tự động quét mã nguồn như TruffleHog hoặc git-secrets vào Git Hooks. Cấu hình này sẽ tự động chặn lệnh git commit nếu phát hiện chuỗi text có định dạng giống mật khẩu, API token hay AWS keys.

**3. Các giải pháp an ninh vật lý & mạng bổ sung (Khuyến nghị)**

Luôn sử dụng VPN (Virtual Private Network): Bật VPN ngay khi kết nối vào Wi-Fi quán cafe để mã hóa toàn bộ luồng dữ liệu truy cập internet, chống lại các cuộc tấn công nghe lén nội mạng.

Dán màn hình chống nhìn trộm (Privacy Filter): Lắp đặt tấm dán từ tính cho màn hình laptop, chỉ cho phép người ngồi đối diện trực tiếp mới nhìn thấy nội dung hiển thị (góc nhìn hẹp 30-60 độ).

Kỷ luật khóa thiết bị: Tạo thói quen sử dụng phím tắt (Win + L trên Windows hoặc Ctrl + Cmd + Q trên macOS) để khóa máy tính trong vòng 1 giây trước khi rời khỏi ghế, dù chỉ là quay lưng lấy nước.

Khóa cáp vật lý (Kensington Lock): Sử dụng dây khóa cáp chống trộm kết nối laptop với chân bàn/ghế cố định nếu làm việc ở khu vực quá đông người.
