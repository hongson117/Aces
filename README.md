# Aces — Hướng Dẫn Tạo & Nhập Tệp Danh Sách Kênh `.json` Bằng AI

**Aces** là trình phát đa phương tiện độc lập (*Neutral Media Player*) dành cho **Android TV / TV Box**, **Điện thoại Android** và **Apple TV (tvOS)**. Ứng dụng không chứa sẵn bất kỳ danh sách kênh truyền hình nào; người dùng hoàn toàn chủ động tự tạo và nạp tệp cấu hình `.json` từ các nguồn luồng hợp pháp mà mình sở hữu.

Tài liệu này hướng dẫn chi tiết cách dùng **AI (ChatGPT, Google Gemini, Claude, Grok...)** để tự động chuyển đổi danh sách link luồng, tên kênh và logo của bạn thành tệp `.json` chuẩn của **Aces** chỉ trong vài giây.

---

## 1. Câu Lệnh Mẫu (Prompt) Dán Cho ChatGPT / Gemini / Claude

Bạn chỉ cần sao chép toàn bộ khung lệnh bên dưới, dán vào **ChatGPT** hoặc **Gemini**, sau đó dán danh sách kênh/link/logo thô của bạn xuống cuối cùng:

```text
Hãy đóng vai chuyên gia xử lý dữ liệu JSON. Dựa trên danh sách kênh, mã luồng và logo tôi cung cấp ở cuối bài, hãy xuất ra DUY NHẤT 1 khối mã JSON hợp lệ (không giải thích thừa) theo đúng chuẩn schemaVersion 2 của ứng dụng Aces như sau:

{
  "app": "Aces",
  "schemaVersion": 2,
  "channels": [
    {
      "id": "ma_kenh_khong_dau_duy_nhat",
      "name": "Tên hiển thị của kênh",
      "group": "Tên nhóm tab (ví dụ: Thể Thao, Phim Truyện, Tin Tức...)",
      "sourceType": "id",
      "contentId": "40_ky_tu_hex_neu_la_acestream_id",
      "infohash": "40_ky_tu_hex_neu_la_magnet_btih_infohash",
      "directUrl": "https://...m3u8_neu_la_luong_hls_truc_tiep",
      "backups": [
        "40_ky_tu_hex_luong_du_phong_1",
        "40_ky_tu_hex_luong_du_phong_2"
      ],
      "backupTypes": ["id", "infohash"],
      "resolution": "1080p FHD",
      "fps": "50 FPS",
      "is4K": false,
      "logo": "https://duong-dan-anh-logo.png"
    }
  ]
}

Quy tắc bắt buộc khi tạo JSON cho Aces:
1. Nhận diện đúng loại nguồn (`sourceType`):
   - Nếu nguồn là `acestream://<40_hex>` hoặc mã Content ID 40 ký tự hex -> đặt `"sourceType": "id"` và điền 40 ký tự chữ thường vào `"contentId"`.
   - Nếu nguồn là `magnet:?xt=urn:btih:<40_hex>` hoặc Infohash -> đặt `"sourceType": "infohash"` và điền 40 ký tự chữ thường vào `"infohash"`.
   - Nếu nguồn là link HTTP/HTTPS trực tiếp (`.m3u8`, `.ts`, `.mp4`) -> đặt `"sourceType": "direct"` và điền URL vào `"directUrl"`.
2. Nếu 1 kênh có nhiều link/mã trùng tên -> lấy mã đầu tiên làm luồng chính, các mã 40-hex còn lại gom vào mảng `"backups"` kèm `"backupTypes"` tương ứng (`"id"` hoặc `"infohash"`).
3. Trường `"id"` chỉ dùng chữ thường `a-z`, số `0-9` và dấu gạch dưới `_`, bắt đầu bằng chữ cái, không trùng lặp.
4. Nếu tên kênh có chữ `4K` hoặc `UHD` -> đặt `"is4K": true` và `"resolution": "3840x2160 UHD"`.
5. Giữ nguyên thứ tự nhóm `"group"` để Aces tự động chia thành các Tab trên màn hình TV và Điện thoại.

Danh sách nguồn thô của tôi như sau:
[DÁN DANH SÁCH TÊN KÊNH, MÃ LUỒNG VÀ LINK LOGO CỦA BẠN VÀO ĐÂY]
```

Sau khi AI trả kết quả:
- **Cách 1 (Dán trực tiếp):** Sao chép đoạn JSON kết quả -> mở **Aces** -> **📂 Nhập / Xuất JSON** -> **📋 Dán nội dung JSON**.
- **Cách 2 (Lưu thành tệp `.json`):** Nhờ AI *"Xuất kết quả trên thành file `channels.json` để tôi tải về"* (hoặc lưu vào Notepad với đuôi `.json`, mã hóa `UTF-8`).

---

## 2. Giải Thích Chi Tiết Từng Trường Trong Tệp `.json`

| Trường | Bắt buộc | Mô tả chi tiết |
| --- | :---: | --- |
| `name` | **Có** | Tên kênh hiển thị trên thẻ kênh và thanh phát (VD: `"Open Movie 1080p"`). |
| `sourceType` | Khuyến nghị | `"id"` (Content ID 40 hex), `"infohash"` (BitTorrent Infohash 40 hex), hoặc `"direct"` (link `.m3u8` / HTTP trực tiếp). Nếu bỏ trống, Aces tự nhận diện. |
| `contentId` | 1 trong 3 | Chuỗi **40 ký tự hex** (`0-9`, `a-f`) của Content ID (chấp nhận cả dạng `acestream://...`). |
| `infohash` | 1 trong 3 | Chuỗi **40 ký tự hex** Infohash (chấp nhận cả dạng `magnet:?xt=urn:btih:...`). |
| `directUrl` | 1 trong 3 | Đường dẫn `http://` hoặc `https://` phát trực tiếp HLS `.m3u8` / `.ts` / `.mp4`. |
| `group` | Tùy chọn | Tên nhóm kênh. Aces tự động tạo các thanh **Tab lọc nhóm** theo đúng thứ tự xuất hiện của `group`. |
| `backups` | Tùy chọn | Mảng các mã 40-hex dự phòng `["hex1", "hex2"]`. Khi luồng chính bận hoặc ngắt, Aces tự động chuyển sang luồng dự phòng hoặc bấm nút **Đổi nguồn (1/N)** trên màn hình phát. |
| `backupTypes` | Tùy chọn | Mảng loại tương ứng với `backups` (VD: `["id", "infohash"]`). Mặc định là `"id"`. |
| `resolution` | Tùy chọn | Độ phân giải hiển thị nhãn (VD: `"1080p FHD"`, `"720p HD"`, `"3840x2160 UHD"`). |
| `fps` | Tùy chọn | Tốc độ khung hình hiển thị trên thẻ kênh (VD: `"50 FPS"`, `"60 FPS"`, `"25 FPS"`). |
| `is4K` | Tùy chọn | `true` nếu là kênh 4K UHD (hiển thị huy hiệu vàng `4K` và tự động tăng bộ đệm chống giật). |
| `logo` | Tùy chọn | Link ảnh logo `https://...` (PNG, WebP, JPG, SVG). Ngoài ra có thể kéo-thả ảnh logo riêng từ máy tính qua cổng Studio `:8100`. |

---

## 3. Bốn Cách Đưa Tệp `.json` Vào Ứng Dụng Aces

1. **Qua Trình Duyệt Mạng LAN — Aces Studio `:8100` (Tiện nhất cho Android TV & Apple TV):**
   - Mở **Aces** trên TV, xem địa chỉ dạng `http://192.168.1.xxx:8100` (hoặc quét mã QR trên màn hình TV).
   - Mở địa chỉ đó trên trình duyệt Điện thoại hoặc Máy tính cùng Wi-Fi -> kéo-thả tệp `.json` vào ô **Nhập JSON**.
   - Toàn bộ kênh và logo sẽ hiển thị ngay lập tức trên TV và **tự động lưu vĩnh viễn** cho mọi lần mở sau.
2. **Chọn Tệp `.json` Trực Tiếp Trên Thiết Bị:**
   - Bấm **📂 Nhập / Xuất JSON** -> **📁 Chọn file `.json` trên thiết bị** -> chọn tệp từ thư mục `Downloads` hoặc USB.
3. **Tải Từ Đường Dẫn URL `.json`:**
   - Lưu tệp `.json` của bạn lên GitHub Gist (Raw URL), Google Drive direct link, Cloudflare R2 hoặc máy chủ cá nhân -> vào **🌐 Nhập từ đường dẫn URL (`.json`)** và dán link `https://...`.
4. **Dán Trực Tiếp Từ Bộ Nhớ Tạm (Clipboard):**
   - Sao chép đoạn JSON do AI vừa tạo (hỗ trợ cả khối ```` ```json ... ``` ````) -> bấm **📋 Dán nội dung JSON**.

---

## 4. Tệp JSON Mẫu Hợp Pháp (CC-BY 3.0 Open Movies)

Bạn có thể tải tệp mẫu [`aces_channel_template.json`](./aces_channel_template.json) hoặc dùng đường dẫn Raw trực tiếp bên dưới để kiểm tra thử tính năng nhập kênh của Aces:

```text
https://raw.githubusercontent.com/hongson117/Aces/main/aces_channel_template.json
```

---

## 5. Khuyến Cáo Pháp Lý & Quyền Riêng Tư

- [Khuyến cáo Pháp lý & Trách nhiệm Người dùng (`DISCLAIMER.md`)](./DISCLAIMER.md)
- [Chính sách Quyền riêng tư (`PRIVACY_POLICY.md`)](./PRIVACY_POLICY.md)
