# 🇻🇳 Dashboardphone — Hướng dẫn ARTEMIS bằng tiếng Việt

> **ARTEMIS** là hệ thống tác tử AI tự động hóa thao tác trên điện thoại Android, có thể tiếp nhận yêu cầu bằng ngôn ngữ tự nhiên, quan sát màn hình, thực hiện thao tác, kiểm tra kết quả và lưu bằng chứng thực thi.

[🌐 Trang giới thiệu tiếng Việt](https://xulytiengviet.github.io/Dashboardphone/) · [Mã nguồn của nhánh Dashboardphone](https://github.com/xulytiengviet/Dashboardphone) · [ARTEMIS gốc (google/artemis)](https://github.com/google/artemis) · [Tài liệu tiếng Anh](./README.md)

![Minh họa ARTEMIS](./docs/assets/artemis-banner.png)

## 1. ARTEMIS làm được gì?

- **Quan sát và hiểu màn hình:** đọc cây giao diện qua Android Accessibility/UIAutomator, sử dụng ảnh chụp và định vị dựa trên hình ảnh khi cần.
- **Thực hiện thao tác:** chạm, vuốt, cuộn, nhập liệu, mở ứng dụng, điều hướng nhiều màn hình.
- **Xác minh và xử lý lỗi:** đọc lại màn hình sau các hành động, phản hồi lỗi thao tác, hỗ trợ chiến lược phục hồi.
- **Thu thập bằng chứng:** ảnh chụp màn hình, nhật ký thực thi, lịch sử tác vụ và dữ liệu chẩn đoán Logcat.
- **Điều khiển Android thật hoặc emulator** thông qua môi trường được thiết lập trên máy chủ/PC.
- **Kết nối MCP** để trợ lý lập trình như Codex, Claude Code, Antigravity, Cursor, Windsurf hoặc VS Code có thể gọi công cụ Android.

Các khả năng cụ thể phụ thuộc thiết bị, phiên bản Android, ứng dụng, cấu hình mô hình và quyền cấp cho hệ thống.

## 2. Phân biệt trang giới thiệu và hệ thống thực

| Thành phần | Nhiệm vụ | Có điều khiển điện thoại thật? |
| --- | --- | --- |
| `index.html` (GitHub Pages) | Trang Việt hóa, giới thiệu kiến trúc và mô phỏng 5 bước | **Không** |
| `apps/showcase_ui` | Giao diện Angular cho các phiên thao tác ARTEMIS | **Có**, khi backend được chạy và thiết bị đã kết nối |
| `apps/admin_console` | Dashboard và API backend | **Có**, trong điều kiện đã cấu hình |
| `artemis/` và `mcp_server/` | Bộ điều khiển, tác tử và giao tiếp công cụ | **Có**, qua runtime phù hợp |

**Lưu ý:** GitHub Pages là web tĩnh. Tự nó không có khả năng chạy ADB, chụp màn hình điện thoại qua USB hoặc tự điều khiển Android. Không nên xem các bước mô phỏng như log chạy thật.

## 3. Cài đặt trên Windows

Yêu cầu: thiết bị Android đã bật **Tùy chọn nhà phát triển / Gỡ lỗi USB (USB Debugging)** hoặc Android Emulator; môi trường máy tính có kết nối phù hợp.

Mở **PowerShell**:

```powershell
git clone https://github.com/xulytiengviet/Dashboardphone.git
cd Dashboardphone
.\start.bat
```

Với **Command Prompt**, chạy `start.bat` (không có `.\`).

Trình khởi động có thể hỗ trợ phát hiện và thiết lập ADB, scrcpy, FFmpeg và `uv`. Làm theo thông báo trên terminal và chỉ chấp thuận các quyền/cài đặt bạn hiểu rõ.

## 4. Cài đặt trên macOS/Linux

```bash
git clone https://github.com/xulytiengviet/Dashboardphone.git
cd Dashboardphone
./start.sh
```

Sau khi chạy, giao diện runtime thường có thể mở tại **http://localhost:8000** (đây là địa chỉ chạy **trên máy của bạn**, không phải GitHub Pages). Nếu sử dụng giao diện Angular trong chế độ phát triển:

```bash
cd apps/showcase_ui
npm install
npm start
```

Giao diện Angular mặc định ở **http://localhost:4200** và proxy về backend. Các bước thiết lập bổ sung theo yêu cầu và phiên bản Node.js trong `apps/showcase_ui/README.md`.

## 5. Kết nối với trợ lý AI qua MCP

Trong thư mục dự án đã cấu hình môi trường:

```bash
uv run artemis mcp --install all
```

Ví dụ cài cho từng trợ lý:

```bash
uv run artemis mcp --install codex
uv run artemis mcp --install antigravity
```

MCP là giao diện cho công cụ AI. MCP **không tự cấp quyền điều khiển** khi thiếu thiết bị Android, backend/ADB, mô hình hoặc cấu hình phù hợp. Nếu máy đang dùng chính sách bảo mật doanh nghiệp, hãy xác nhận quyền dùng thiết bị và dữ liệu trước khi bật.

## 6. Thử tác vụ mẫu

```bash
uv run artemis run "Mở phần Cài đặt, tìm mục Pin và cho tôi biết mức pin đang hiển thị" --profile flash
```

Tác vụ minh họa khác: “Mở ứng dụng thử nghiệm, nhập dữ liệu giả định, kiểm tra thông báo thành công và chụp ảnh màn hình kết quả.”

- **Flash:** phản ứng nhanh qua vòng lặp quan sát–hành động, thích hợp cho tác vụ tương đối trực tiếp.
- **Pro:** tăng cường kiểm tra điều kiện, xử lý trường hợp khó và phục hồi khi không thể thao tác như mong đợi.

Thời gian Flash **3–5 giây/bước** được dự án công bố là giá trị điển hình, không phải cam kết. Kết quả còn phụ thuộc thiết bị, mạng, nhà cung cấp mô hình và độ phức tạp của tác vụ.

## 7. Chỉ số AndroidWorld: hiểu đúng

Theo [README chính thức của google/artemis](https://github.com/google/artemis), nhóm dự án báo cáo tỷ lệ hoàn thành **99%+** trong bài đánh giá **AndroidWorld**, trên hơn 100 tác vụ nhiều bước. Đây là **kết quả tự công bố**, không đại diện cho tỷ lệ thành công tuyệt đối trên tất cả thiết bị và ứng dụng ngoài môi trường benchmark. [Bộ đánh giá AndroidWorld của Google Research](https://github.com/google-research/android_world) là một dự án riêng.

## 8. Nguyên tắc bảo mật

1. Dùng **thiết bị thử nghiệm** và **tài khoản thử nghiệm**; hạn chế chạy trên máy chứa dữ liệu tài chính hoặc cá nhân.
2. Kiểm tra quyền **ADB/USB Debugging** và **Trợ năng (Accessibility)**, gỡ helper và tắt quyền khi không còn sử dụng.
3. Không đưa mật khẩu, OTP, token truy cập, khóa API hoặc dữ liệu bí mật vào yêu cầu thao tác hay ảnh chụp công khai.
4. Ảnh màn hình, nội dung giao diện hoặc lịch sử có thể đi qua mô hình AI bên ngoài tùy cấu hình — đọc chính sách của nhà cung cấp mô hình.
5. Không mở công khai cổng backend/daemon qua Internet nếu không có xác thực và bảo vệ mạng. Các tài liệu ARTEMIS cảnh báo API quản trị daemon hiện không có xác thực từ xa tích hợp; ưu tiên SSH tunnel/VPN hoặc proxy được cấu hình an toàn.
6. Xem kỹ từng thao tác có thể làm thay đổi dữ liệu; yêu cầu xác nhận thủ công cho giao dịch, xóa dữ liệu hoặc thao tác nhạy cảm.

## 9. Cấu trúc mã nguồn

```text
Dashboardphone/
├── index.html                  # Trang giới thiệu VI, tĩnh
├── README_VI.md                # Tài liệu này
├── README.md                   # Tài liệu tiếng Anh gốc
├── apps/
│   ├── admin_console/         # Backend UI/API
│   └── showcase_ui/           # Giao diện Angular
├── artemis/                   # Agent, runtime, drivers, tools
├── mcp_server/                # Công cụ MCP
├── start.bat                  # Khởi động Windows
└── start.sh                   # Khởi động macOS/Linux
```

## Xuất bản trang giới thiệu bằng GitHub Pages

Kho đã có `index.html` tại thư mục gốc và workflow `.github/workflows/pages.yml` để đóng gói, xuất bản trang HTML tĩnh. **Cần bật GitHub Pages một lần trong cấu hình kho**, nếu chưa bật:

1. Mở **Settings → Pages** của kho `xulytiengviet/Dashboardphone`.
2. Ở **Build and deployment → Source**, chọn **GitHub Actions**.
3. Mở **Actions → Đăng trang ARTEMIS tiếng Việt**, chọn **Run workflow** để xuất bản lại.
4. Sau khi workflow hoàn thành thành công, kiểm tra trang tại https://xulytiengviet.github.io/Dashboardphone/.

Nếu Pages chưa bật, bước **Cấu hình GitHub Pages** sẽ báo `Get Pages site failed: Not Found`. Đây là lỗi cài đặt Pages, không phải lỗi HTML của bản giao diện. GitHub App không thể tự bật Pages khi thiếu quyền quản trị đặc biệt. Xem [GitHub Pages settings](https://github.com/xulytiengviet/Dashboardphone/settings/pages).

## 10. Nguồn, ghi nhận tác giả và giấy phép

- Mã nguồn upstream: **[google/artemis](https://github.com/google/artemis)**.
- Bộ benchmark: **[google-research/android_world](https://github.com/google-research/android_world)**.
- ARTEMIS sử dụng giấy phép **Apache License 2.0** theo tệp [LICENSE](./LICENSE). Giữ nguyên thông báo bản quyền và giấy phép khi phân phối mã nguồn phái sinh.
- Trang giao diện tiếng Việt trên đây là đóng góp độc lập của kho **xulytiengviet/Dashboardphone**, **không phải trang sản phẩm chính thức hoặc bản chứng nhận của Google**.

---
*Tài liệu tiếng Việt: cập nhật ngày 21/10/2026. Đối chiếu tài liệu upstream khi cài hoặc thay đổi phiên bản.*
