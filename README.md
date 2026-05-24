# Hướng dẫn Build & Sideload WebDriverAgent cho iOS 17.5.1 (Windows)

Dự án này chứa mã nguồn **WebDriverAgent (WDA)** và một GitHub Actions workflow giúp tự động biên dịch dự án thành file `.ipa` không ký (unsigned) trên macOS runner của GitHub. 

Sau khi tải về file `.ipa`, bạn có thể dễ dàng ký (sign) bằng Apple ID cá nhân của mình và cài đặt lên thiết bị iOS 17.5.1 bằng các công cụ trên Windows như **Sideloadly** hoặc **AltStore**.

---

## 🚀 Bước 1: Đẩy mã nguồn lên GitHub của bạn

Vì bạn đang dùng Windows và không thể tự build file iOS trực tiếp, chúng ta sẽ đẩy dự án này lên GitHub cá nhân để chạy build bằng macOS runner miễn phí.

1. Mở terminal (PowerShell hoặc CMD) tại thư mục dự án `C:\source_code_new\WebDriverAgent_Build`.
2. Đăng nhập vào tài khoản GitHub của bạn hoặc đảm bảo bạn đã cấu hình Git.
3. Tạo một repository mới trống trên GitHub (ví dụ: `WebDriverAgent_Build`).
4. Chạy các lệnh sau để đẩy code lên GitHub của bạn:
   ```bash
   git add .
   git commit -m "feat: setup WDA build workflow for iOS 17.5.1"
   git branch -M main
   git remote add origin https://github.com/TÊN_USER_GITHUB_CỦA_BẠN/TÊN_REPO.git
   git push -u origin main
   ```
   *(Thay `TÊN_USER_GITHUB_CỦA_BẠN` và `TÊN_REPO` bằng thông tin của bạn).*

---

## 🛠️ Bước 2: Chạy Build & Tải File `.ipa`

Khi bạn đẩy code lên GitHub:
1. GitHub Actions sẽ tự động phát hiện file `.github/workflows/build-wda.yml` và kích hoạt quá trình build.
2. Bạn truy cập vào tab **Actions** trên repository GitHub của mình.
3. Chọn workflow **Build WebDriverAgent** đang chạy.
4. Chờ khoảng 3-5 phút cho quá trình build hoàn tất.
5. Sau khi thành công, cuộn xuống phần **Artifacts** ở cuối trang và tải file **`WebDriverAgentRunner-IPA`** về máy tính Windows của bạn. giải nén ra bạn sẽ nhận được file `WebDriverAgentRunner.ipa`.

---

## 📲 Bước 3: Sideload cài đặt vào iPhone (iOS 17.5.1) trên Windows

Có 2 công cụ phổ biến nhất trên Windows để tự động ký và cài đặt file `.ipa` thông qua Apple ID của bạn:

### Lựa chọn A: Dùng Sideloadly (Khuyên dùng - Rất nhanh)
1. Tải và cài đặt **[Sideloadly](https://sideloadly.io/)** trên máy tính Windows.
2. Đảm bảo bạn đã cài đặt **iTunes** phiên bản chính thức (không phải bản từ Microsoft Store) và **iCloud** trên Windows để máy nhận diện được iPhone.
3. Kết nối iPhone của bạn với máy tính qua cáp USB và nhấn **"Tin cậy máy tính này" (Trust this computer)** trên màn hình điện thoại.
4. Mở Sideloadly:
   * **Device:** Chọn thiết bị iPhone của bạn.
   * **Apple Account:** Nhập email Apple ID của bạn.
   * **IPA:** Kéo thả file `WebDriverAgentRunner.ipa` đã tải về vào ô này.
5. Nhấn **Start**, nhập mật khẩu Apple ID nếu được yêu cầu (Sideloadly gửi trực tiếp đến máy chủ Apple để xin chứng chỉ phát triển).
6. Chờ thông báo **Done** hoàn tất. Ứng dụng `WebDriverAgentRunner` sẽ xuất hiện trên màn hình điện thoại của bạn.

### Lựa chọn B: Dùng AltStore
1. Tải và cài đặt **[AltStore](https://altstore.io/)** (chạy AltServer trên Windows).
2. Kết nối iPhone với máy tính và cài đặt AltStore lên iPhone thông qua AltServer.
3. Gửi file `WebDriverAgentRunner.ipa` vào điện thoại (qua AirDrop, Google Drive hoặc lưu trữ nội bộ).
4. Mở ứng dụng **AltStore** trên iPhone, vào tab **My Apps**, nhấn dấu **`+`** ở góc trên cùng và chọn file `WebDriverAgentRunner.ipa` để tiến hành cài đặt và tự động ký.

---

## ⚙️ Bước 4: Cấu hình trên iPhone (iOS 17.5.1)

Do đây là ứng dụng tự ký phát triển cá nhân, bạn phải bật các quyền bảo mật trên iOS thì WDA mới có thể khởi chạy:

1. **Tin cậy chứng chỉ (Trust Developer):**
   * Truy cập **Cài đặt (Settings) > Cài đặt chung (General) > Quản lý VPN & Thiết bị (VPN & Device Management)**.
   * Dưới phần *Ứng dụng nhà phát triển (Developer App)*, chọn Apple ID của bạn và bấm **Tin cậy (Trust)**.

2. **Bật chế độ nhà phát triển (Developer Mode) - Bắt buộc trên iOS 16+:**
   * Truy cập **Cài đặt (Settings) > Quyền riêng tư & Bảo mật (Privacy & Security)**.
   * Cuộn xuống dưới cùng tìm mục **Chế độ nhà phát triển (Developer Mode)**.
   * Gạt bật sang **On** và làm theo hướng dẫn của hệ thống (điện thoại sẽ tự khởi động lại).
   * Sau khi khởi động lại, chọn **Bật (Turn On)** và nhập mật khẩu khóa màn hình của bạn để xác nhận.

---

## 🤖 Bước 5: Kết nối Appium với WebDriverAgent đã cài

Sau khi cài thành công WDA lên điện thoại, khi chạy Appium kiểm thử từ Windows, bạn cấu hình các **Desired Capabilities** như sau để Appium sử dụng bản WDA đã cài đặt sẵn mà không cần cố gắng biên dịch lại (vì Windows không biên dịch được):

```json
{
  "platformName": "iOS",
  "appium:platformVersion": "17.5.1",
  "appium:deviceName": "Tên_Thiết_Bị_Của_Bạn",
  "appium:automationName": "XCUITest",
  "appium:udid": "Mã_UDID_Của_Thiết_Bị",
  "appium:usePreinstalledWDA": true,
  "appium:updatedWDABundleId": "com.facebook.WebDriverAgentRunner"
}
```
*Lưu ý: Sideloadly / AltStore khi cài đặt sẽ đổi bundle ID của app sang Apple ID của bạn, nếu bạn gặp lỗi không nhận diện được app, hãy kiểm tra Bundle ID của WDA đã cài (Sideloadly thường hiển thị Bundle ID sau khi cài xong, ví dụ: `com.username.WebDriverAgentRunner` hoặc tương tự) và điền vào capability `updatedWDABundleId`.*
