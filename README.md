# Website Văn Diễn – VD SOLUTIONS

Website giới thiệu song ngữ (Việt/Anh) cho thương hiệu cá nhân **Văn Diễn – VD SOLUTIONS**: tiếp nhận yêu cầu mua sản phẩm và thực hiện dịch vụ. Mục tiêu chính của trang là để khách **để lại thông tin và yêu cầu**.

- **Một file duy nhất:** `index.html` (HTML, CSS, JavaScript và logo nằm chung trong file, không cần cài đặt hay build).
- **Không dùng framework.** Chỉ cần tải file lên bất kỳ hosting tĩnh nào là chạy.

---

## 1. Cấu trúc trang

| Địa chỉ | Trang |
|---|---|
| `#home` | Trang chủ |
| `#products` | Sản phẩm (8 danh mục) |
| `#services` | Dịch vụ (danh sách 10 nhóm) |
| `#svc-design` `#svc-ads` `#svc-web` `#svc-tech` `#svc-office` `#svc-build` `#svc-ship` `#svc-event` `#svc-partner` `#svc-other` | Trang riêng của từng dịch vụ |
| `#find` | Tìm sản phẩm (form) |
| `#request` | Gửi yêu cầu dịch vụ (form) |
| `#about` | Giới thiệu |
| `#partners` | Đối tác |
| `#news` | Tin tức |
| `#contact` | Liên hệ (form) |
| `#privacy` | Quyền riêng tư |

Có sẵn trên mọi trang:
- Dải tin VnExpress nằm dưới thanh menu.
- Nút **Gọi** và nút **Chat Zalo** cố định ở góc dưới bên phải.
- Nút chuyển ngôn ngữ **VI / EN** (lựa chọn của khách được nhớ trong trình duyệt).

---

## 2. Sửa thông tin nhanh

Mở `index.html`, tìm đoạn đầu của thẻ `<script>`. Các giá trị cần sửa nằm ở đây:

| Biến | Ý nghĩa | Giá trị hiện tại |
|---|---|---|
| `ZALO_URL` | Liên kết nút Chat Zalo | `https://zalo.me/0354830717` |
| `PHONE_FMT` / `PHONE_TEL` | Số hiển thị / số gọi của nút Gọi | `0388 821 889` / `0388821889` |
| `EMAIL` | Email liên hệ | `vandien717@gmail.com` |
| `ADDRESS` | Địa chỉ văn phòng | `Mỹ Phước 1, Bến Cát, TP. Hồ Chí Minh` |
| `PERSON` | Người phụ trách | `Võ Văn Diễn` |
| `AREA` | Khu vực phục vụ (để trống thì trang hiện câu hướng dẫn khách ghi địa điểm trong yêu cầu) | trống |
| `FORM_ENDPOINT` | Địa chỉ nhận form (xem mục 4) | trống |

Nội dung sản phẩm và dịch vụ nằm trong hai danh sách `PRODUCTS` và `SERVICES`. Mỗi mục có tên, mô tả và phạm vi bằng cả tiếng Việt lẫn tiếng Anh, dạng `["tiếng Việt","English"]`. Sửa hoặc thêm một mục là trang tự cập nhật, kể cả menu và danh sách.

Văn bản của từng trang nằm trong khối `PAGES`. Mỗi chỗ có cặp `L("tiếng Việt","English")`, **sửa cả hai vế** để hai ngôn ngữ luôn khớp nhau.

---

## 3. Giao diện

- **Màu:** lấy từ logo: đỏ `#E3000F`, vàng `#FDC102`, xanh lá `#07883A`. Khai báo ở khối `:root` đầu phần `<style>`. Có sẵn chế độ sáng và tối theo cài đặt máy của khách.
- **Chữ:** Bricolage Grotesque (tiêu đề), Be Vietnam Pro (nội dung), JetBrains Mono (nhãn nhỏ), tải từ Google Fonts.
- **Logo:** nhúng trực tiếp trong file (ảnh PNG nền trong suốt). Muốn đổi logo, thay chuỗi `data:image/png;base64,...` ở biến `LOGO`.
- **Điện thoại:** đã thiết kế cho màn hình nhỏ (menu thu gọn thành nút ba gạch).

---

## 4. Form nhận yêu cầu

Có 4 form: yêu cầu nhanh (trang chủ), **Tìm sản phẩm**, **Gửi yêu cầu dịch vụ**, **Liên hệ**. Form kiểm tra họ tên, số điện thoại Việt Nam, email và ô đồng ý trước khi gửi.

**Hiện tại form chưa tự gửi về cho anh.** Khách điền xong sẽ nhận nội dung soạn sẵn, bấm chép rồi gửi qua email hoặc Zalo. Đây là giải pháp tạm vì trang chưa có nơi nhận dữ liệu.

### Cách nối form vào Google Sheets (miễn phí)

1. Tạo một Google Sheet mới.
2. Chọn **Tiện ích mở rộng → Apps Script**, dán đoạn sau:

```javascript
function doPost(e) {
  var d = JSON.parse(e.postData.contents);
  SpreadsheetApp.getActiveSheet().appendRow([
    new Date(), d.kind, d.name || "", d.phone || "", d.email || "",
    d.company || "", JSON.stringify(d)
  ]);
  return ContentService.createTextOutput("ok");
}
```

3. Chọn **Triển khai → Triển khai mới → Ứng dụng web**, đặt quyền truy cập là **Bất kỳ ai**, rồi sao chép địa chỉ web app.
4. Dán địa chỉ đó vào biến `FORM_ENDPOINT` trong `index.html`.
5. Gửi thử một yêu cầu và kiểm tra Sheet có dòng mới.

Dữ liệu gửi đi gồm: `kind` (loại form), `lang`, `time` và các trường khách điền.

---

## 5. Dải tin VnExpress

- Mặc định dải tin hiện các **chuyên mục** của VnExpress, bấm vào mở trang gốc ở tab mới.
- Khi web chạy trên hosting riêng, dải tin tự lấy **tiêu đề bài mới** từ mục Số hóa qua dịch vụ trung gian `api.rss2json.com`. Nếu không lấy được thì quay về hiện chuyên mục.
- Đổi chuyên mục nguồn: sửa biến `NEWS_FEED` (ví dụ `https://vnexpress.net/rss/kinh-doanh.rss`).
- Trang chỉ dẫn liên kết, không sao chép nội dung bài. Bản quyền thuộc VnExpress.

---

## 6. Đưa website lên mạng

Bộ file đưa lên gồm 7 file đặt chung một thư mục (không để trong thư mục con):

| File | Vai trò |
|---|---|
| `index.html` | Toàn bộ website |
| `vercel.json` | Cấu hình Vercel và bảo mật |
| `manifest.json` | Cho phép khách thêm website vào màn hình chính điện thoại như một ứng dụng |
| `icon-192.png`, `icon-512.png`, `icon-maskable-512.png` | Biểu tượng ứng dụng (logo VD) |
| `apple-touch-icon.png` | Biểu tượng khi thêm vào màn hình chính trên iPhone |

`manifest.json` đặt tên ứng dụng là "Văn Diễn", mở ở chế độ toàn màn hình, và có 3 lối tắt khi bấm giữ biểu tượng: Tìm sản phẩm, Gửi yêu cầu dịch vụ, Liên hệ. Muốn đổi tên hoặc màu, sửa các trường `name`, `short_name`, `theme_color`, `background_color`.

### Cách 1: Vercel (đề xuất)

1. Tạo tài khoản tại vercel.com (có gói miễn phí).
2. Đặt `index.html` và `vercel.json` vào một thư mục, đẩy lên một kho GitHub, rồi chọn **Add New → Project** và nhập kho đó. Không cần chọn framework (chọn **Other**), không cần lệnh build.
3. Hoặc dùng dòng lệnh: trong thư mục chứa hai file, chạy `npx vercel --prod`.
4. Vào **Settings → Domains** để trỏ tên miền riêng về dự án.
5. Mở trang trên điện thoại thật và kiểm tra: nút Gọi, nút Chat Zalo, form, chuyển VI/EN, dải tin.

### Tệp `vercel.json` làm gì

- Bật `cleanUrls` (địa chỉ không có đuôi `.html`).
- Thêm các tiêu đề bảo mật: chặn nhúng trang vào khung của web khác, chặn đoán sai kiểu file, giới hạn thông tin nguồn gửi đi, tắt camera, micro, vị trí.
- Thêm **Content-Security-Policy** (CSP) chỉ cho phép trang tải: Google Fonts, dịch vụ tin `api.rss2json.com` và Google Apps Script (`script.google.com`, `script.googleusercontent.com`) để nhận form.

**Quan trọng:** nếu sau này anh thêm dịch vụ bên ngoài (ví dụ Google Analytics, địa chỉ nhận form khác, ảnh lấy từ web khác), phải thêm tên miền của dịch vụ đó vào CSP trong `vercel.json`, nếu không trình duyệt sẽ chặn. Ví dụ ảnh từ `cdn.example.com` thì thêm `https://cdn.example.com` vào `img-src`.

### Cách 2: hosting tĩnh khác

Netlify, Cloudflare Pages hoặc GitHub Pages: chỉ cần tải `index.html` lên. `vercel.json` chỉ dùng cho Vercel, các nơi khác bỏ qua.

---

## 7. Danh sách kiểm tra trước khi công khai

**Nội dung**
- [ ] Đọc và sửa phần tầm nhìn, sứ mệnh, giá trị cốt lõi, phương châm, định hướng (trang Giới thiệu). Đây là bản nháp.
- [ ] Kiểm tra mục "Bên thực hiện" của từng dịch vụ có đúng thực tế không.
- [ ] Bổ sung hình ảnh, thông số, xuất xứ, nhà cung cấp cho sản phẩm khi đã có thông tin xác thực.
- [ ] Thay mục "Yêu cầu tiêu biểu" (đang là ví dụ loại việc) bằng dự án thật khi có và khi khách đồng ý.

**Pháp lý và thương hiệu**
- [ ] Trang đang ghi VD SOLUTIONS là **thương hiệu cá nhân**, chưa có pháp nhân. Khi đăng ký doanh nghiệp, cập nhật chân trang và trang Giới thiệu.
- [ ] Chính sách quyền riêng tư hiện là bản tóm tắt. Vì form thu số điện thoại và email, nên nhờ người am hiểu pháp lý rà lại theo quy định bảo vệ dữ liệu cá nhân.
- [ ] Chỉ đăng tên và logo đối tác khi họ đồng ý bằng văn bản hoặc tin nhắn lưu được.
- [ ] Email và địa chỉ văn phòng đang hiển thị công khai. Xác nhận anh đồng ý.

**Kỹ thuật**
- [ ] Nối form vào Google Sheets (mục 4), gửi thử.
- [ ] Kiểm tra dải tin hiện tiêu đề bài mới sau khi lên hosting riêng.
- [ ] Thêm công cụ đo lượt truy cập (ví dụ Google Analytics) nếu cần, và ghi vào chính sách quyền riêng tư.

---

## 8. Hạn chế đã biết

1. **Tìm kiếm Google:** trang dùng địa chỉ dạng `#products`, nên Google khó đánh chỉ mục từng trang riêng. Nếu SEO là ưu tiên (đặc biệt cho trang Tin tức), nên chuyển sang mỗi trang một địa chỉ riêng như `/san-pham`, `/dich-vu`. Đây là bước nâng cấp tiếp theo hợp lý.
2. **Form chưa tự gửi** cho đến khi nối `FORM_ENDPOINT`.
3. **Tải ảnh từ khách:** form chưa có ô tải ảnh. Khách gửi ảnh sản phẩm qua email hoặc Zalo.
4. **Dịch vụ trung gian lấy tin:** `rss2json.com` là bên thứ ba. Nếu cần ổn định lâu dài, nên tự viết một đoạn lấy tin trên máy chủ riêng.
5. **Liên kết gọi và email** hoạt động tốt nhất trên điện thoại. Trên máy tính, khách vẫn đọc được số và địa chỉ ở trang Liên hệ.

---

© 2026 Văn Diễn – VD SOLUTIONS
