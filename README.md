# Zerotrustdns

Chặn quảng cáo và cờ bạc ở cấp DNS bằng Cloudflare Zero Trust Gateway — miễn phí, không cần cài app hay extension.

Hoạt động với gói miễn phí của Cloudflare (lên đến 300.000 domain bị chặn).

## Cách hoạt động

1. Tải danh sách domain cần chặn từ internet
2. Lọc và loại bỏ trùng lặp, loại bỏ các domain được cho phép
3. Upload lên Cloudflare Gateway dưới dạng "Lists"
4. Tạo Gateway DNS policy chặn toàn bộ các domain trong danh sách
5. Tự cập nhật mỗi ngày lúc 3am

## Blocklist mặc định

- **AdGuard DNS Filter** — chặn quảng cáo
- **hostsVN** — chặn quảng cáo Việt Nam
- **Gambling** — chặn cờ bạc

## Cài đặt

### Bước 1 — Kích hoạt Zero Trust

- Vào [one.dash.cloudflare.com](https://one.dash.cloudflare.com) và đăng nhập
- Làm theo hướng dẫn để kích hoạt gói Zero Trust Free

### Bước 2 — Tạo DNS Location

1. Vào [dash.cloudflare.com](https://dash.cloudflare.com) → sidebar trái chọn **Zero Trust**
2. Vào **Networks → Resolvers & Proxies** → chọn **Add a location**
3. Điền **Location name** tùy thích, bật các endpoint tùy nhu cầu:
   - **IPv4 DNS** — gán IP thẳng vào router/modem nhà mạng
   - **DNS over TLS (DoT)** — gán vào thiết bị Android
   - **DNS over HTTPS (DoH)** — gán vào iPhone/iPad hay trình duyệt máy tính
4. Nên bật thêm **Enable EDNS client subnet** (ECS ẩn danh trong nước) và **Set as Default DNS Location**
5. Ấn **Continue** → **Continue** lần nữa là xong
6. Hệ thống sẽ hiển thị thông số DNS (IPv4, DoT, DoH), bạn gán vào thiết bị theo hướng dẫn

### Bước 3 — Lấy API Token + Account ID

**Account ID:**
- Ở bước 2 bạn đang ở Zero Trust, kéo lên chọn **Overview** — Account ID nằm ở bên phải trang, copy nó

**API Token:**
- Vào [dash.cloudflare.com/profile/api-tokens](https://dash.cloudflare.com/profile/api-tokens)
- Bấm **Create Token → Create Custom Token**
- Đặt tên tùy ý
- Mục **Permissions** chọn: `Zero Trust` → `Edit`
- Bấm **Continue to summary → Create Token**
- Copy token lại (chỉ hiện 1 lần duy nhất)

### Bước 4 — Fork repo

Bấm **Fork → Create fork** ở góc trên bên phải.

### Bước 5 — Thêm secrets vào repo

Vào repo vừa fork → **Settings → Secrets and variables → Actions → New repository secret**

Thêm 2 secrets:
- `CLOUDFLARE_API_TOKEN`
- `CLOUDFLARE_ACCOUNT_ID`

### Bước 6 — Chạy workflow

Vào tab **Actions → Update blocklists → Run workflow**

Chờ khoảng 1-2 phút. Blocklist tự cập nhật mỗi ngày lúc 3am.

## Thêm blocklist tùy chọn

Nếu muốn thêm blocklist khác, vào **Settings → Secrets and variables → Actions → New repository secret**, thêm secret tên `BLOCKLIST_URLS` với nội dung là các URL, mỗi URL một dòng:

```
https://example.com/blocklist1.txt
https://example.com/blocklist2.txt
```

---

Dự án lấy cảm hứng từ [cloudflare-gateway-pihole-scripts](https://github.com/mrrfv/cloudflare-gateway-pihole-scripts) by mrrfv.
