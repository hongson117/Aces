# Aces — AI JSON Playlist Guide (English & Tiếng Việt)

**Aces** is a standalone **Neutral Media Player** for **Android TV / Google TV**, **Android Phones & Tablets**, and **Apple TV (tvOS)**. Aces contains **0 built-in channels** on fresh install; users create and load their own `.json` playlist files from lawful streams they own or are authorized to access.

---

## 🇬🇧 ENGLISH GUIDE — Generate Your `.json` Playlist with AI

Copy the prompt below into **ChatGPT, Google Gemini, or Claude**, and paste your raw stream links, channel names, and logos at the bottom:

```text
Act as a JSON data formatter. Based on the raw list of channel names, stream IDs/links, and logo URLs I paste at the bottom, output ONLY 1 valid JSON object (no extra prose) strictly following schemaVersion 2 for the Aces media player:

{
  "app": "Aces",
  "schemaVersion": 2,
  "channels": [
    {
      "id": "unique_lowercase_channel_id",
      "name": "Channel Display Name",
      "group": "Category Tab Name (e.g. Open Movies, News, Personal...)",
      "sourceType": "id",
      "contentId": "40_hex_chars_if_p2p_content_id",
      "infohash": "40_hex_chars_if_magnet_btih_infohash",
      "directUrl": "https://...m3u8_if_direct_http_stream",
      "backups": ["40_hex_chars_backup_stream_1"],
      "backupTypes": ["id"],
      "resolution": "1080p FHD",
      "fps": "50 FPS",
      "is4K": false,
      "logo": "https://logo-image-url.png"
    }
  ]
}

Strict rules:
1. Detect sourceType accurately:
   - 40-char hex Content ID -> "sourceType": "id", put lowercase hex in "contentId".
   - magnet:?xt=urn:btih:<40_hex> or Infohash -> "sourceType": "infohash", put lowercase hex in "infohash".
   - Direct HTTP/HTTPS URL (.m3u8, .ts, .mp4) -> "sourceType": "direct", put URL in "directUrl".
2. If a channel has multiple streams -> use the first as primary and place remaining 40-hex hashes in "backups" with matching "backupTypes".
3. "id" must use lowercase a-z, 0-9, and underscore _, starting with a letter, unique per channel.
4. Preserve "group" order so Aces automatically creates category tabs on TV & Phone.

Here is my raw stream list:
[PASTE YOUR CHANNEL NAMES, STREAM IDS/URLS, AND LOGO LINKS HERE]
```

### Open-Licensed CC-BY 3.0 Sample Playlist URL
Test Aces immediately with open-source Creative Commons (CC-BY 3.0 Blender Foundation) streams:
```text
https://raw.githubusercontent.com/hongson117/Aces/main/aces_channel_template.json
```

---

## 🇻🇳 HƯỚNG DẪN TIẾNG VIỆT — Tạo Tệp `.json` Bằng AI

Sao chép toàn bộ khung lệnh bên dưới, dán vào **ChatGPT / Gemini / Claude**, sau đó dán danh sách tên kênh, mã luồng và logo của bạn xuống cuối cùng:

```text
Hãy đóng vai chuyên gia xử lý dữ liệu JSON. Dựa trên danh sách kênh, mã luồng và logo tôi cung cấp ở cuối bài, hãy xuất ra DUY NHẤT 1 khối mã JSON hợp lệ (không giải thích thừa) theo đúng chuẩn schemaVersion 2 của ứng dụng Aces như sau:

{
  "app": "Aces",
  "schemaVersion": 2,
  "channels": [
    {
      "id": "ma_kenh_khong_dau_duy_nhat",
      "name": "Tên hiển thị của kênh",
      "group": "Tên nhóm tab (ví dụ: Open Movies, Tin Tức, Cá Nhân...)",
      "sourceType": "id",
      "contentId": "40_ky_tu_hex_neu_la_p2p_content_id",
      "infohash": "40_ky_tu_hex_neu_la_magnet_btih_infohash",
      "directUrl": "https://...m3u8_neu_la_luong_hls_truc_tiep",
      "backups": ["40_ky_tu_hex_luong_du_phong_1"],
      "backupTypes": ["id"],
      "resolution": "1080p FHD",
      "fps": "50 FPS",
      "is4K": false,
      "logo": "https://duong-dan-anh-logo.png"
    }
  ]
}

Quy tắc bắt buộc khi tạo JSON cho Aces:
1. Nhận diện đúng loại nguồn (sourceType):
   - Mã Content ID 40 ký tự hex -> "sourceType": "id", điền vào "contentId".
   - magnet:?xt=urn:btih:<40_hex> hoặc Infohash -> "sourceType": "infohash", điền vào "infohash".
   - Link HTTP/HTTPS trực tiếp (.m3u8, .ts, .mp4) -> "sourceType": "direct", điền vào "directUrl".
2. Nếu 1 kênh có nhiều mã trùng tên -> lấy mã đầu làm luồng chính, các mã 40-hex còn lại đưa vào "backups" kèm "backupTypes".
3. Trường "id" chỉ dùng chữ thường a-z, số 0-9 và gạch dưới _, bắt đầu bằng chữ cái, không trùng lặp.
4. Giữ nguyên thứ tự nhóm "group" để Aces tự động chia thành các Tab trên TV và Điện thoại.

Danh sách nguồn thô của tôi như sau:
[DÁN DANH SÁCH TÊN KÊNH, MÃ LUỒNG VÀ LINK LOGO CỦA BẠN VÀO ĐÂY]
```

---

## Legal, Terms & Privacy (Song ngữ Anh – Việt)

- [Terms of Service / Điều khoản Dịch vụ (`TERMS_OF_SERVICE.md`)](./TERMS_OF_SERVICE.md)
- [Legal Disclaimer / Khuyến cáo Pháp lý (`DISCLAIMER.md`)](./DISCLAIMER.md)
- [Privacy Policy / Chính sách Quyền riêng tư (`PRIVACY_POLICY.md`)](./PRIVACY_POLICY.md)
