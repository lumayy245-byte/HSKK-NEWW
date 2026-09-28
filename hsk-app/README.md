# 兴趣爱好课堂 — Website học tiếng Trung tương tác (HSKK)

Website học tiếng Trung tương tác dành cho người Việt, dựng từ nội dung Bài 1 —
兴趣爱好 (Sở thích). Mỗi bài học gồm **4 phần**: 热身 (Khởi động) → 词汇&语法 (Từ
vựng & Ngữ pháp) → 讲解 (Giảng giải cấu trúc bài) → 练习 (Luyện tập) — di chuyển tự
do giữa các tab, không bắt buộc theo thứ tự. Toàn bộ nội dung chữ Hán được giữ
nguyên từ tài liệu gốc; pinyin và nghĩa tiếng Việt do 鹿长老 biên soạn thêm.

Không cần cài đặt, không cần build — chỉ dùng HTML/CSS/JavaScript thuần.

## 1. Cách chạy trên máy tính — ĐỌC KỸ MỤC NÀY

Bản này có tính năng **ghi âm bằng micro ngay trên web** (phần 热身 và 练习), nên
**bắt buộc phải mở qua local server** (`http://localhost...`) — nếu double-click mở
thẳng file `index.html` (`file://...`), trình duyệt sẽ CHẶN quyền micro và nút ghi
âm sẽ báo lỗi. Các phần khác của trang vẫn chạy được khi mở trực tiếp, nhưng để
dùng đủ tính năng, hãy luôn chạy qua server theo 1 trong 2 cách sau:

**Cách 1 — nếu máy đã cài Python** (hầu hết máy Mac/Linux có sẵn):
```bash
cd đường-dẫn-tới/hsk-app
python3 -m http.server 8080
```
rồi mở `http://localhost:8080` trên trình duyệt.

**Cách 2 — nếu dùng VS Code:** cài extension **Live Server**, mở thư mục `hsk-app`
trong VS Code, chuột phải vào `index.html` → "Open with Live Server".

> Trình duyệt khuyên dùng: **Chrome hoặc Edge bản mới** — có giọng đọc tiếng Trung
> zh-CN sẵn cho nút 🔊, và hỗ trợ ghi âm ổn định nhất. Khi bấm "🎙️ Bắt đầu" lần đầu,
> trình duyệt sẽ hỏi quyền dùng micro — nhớ bấm **Cho phép/Allow**.

## 2. Cấu trúc thư mục

```
hsk-app/
├── index.html                ← file mở đầu tiên
├── css/
│   └── style.css             ← toàn bộ design system (màu, layout, component)
├── js/
│   ├── data/
│   │   ├── course.js         ← danh sách 14 bài học (đăng ký bài mới ở đây)
│   │   └── lessons/
│   │       └── lesson1.js    ← nội dung Bài 1: warmup / vocabGrammar / explanation / practice
│   ├── lib/
│   │   ├── state.js          ← state toàn app (XP, tiến độ theo 4 phần...)
│   │   ├── speak.js          ← engine phát âm (audio thật hoặc giọng đọc máy)
│   │   └── videoMap.js       ← link video hướng dẫn đã host sẵn cho phần 讲解
│   ├── components/
│   │   ├── ui.js             ← khối UI dùng chung (chữ Hán, mascot, XP toast...)
│   │   ├── recorder.js       ← component đếm ngược + ghi âm (dùng cho 热身 & 练习)
│   │   └── videoPanel.js     ← component tải/dán video hướng dẫn (dùng cho 讲解)
│   ├── views.js               ← trang chủ / khoá học / tiến độ
│   ├── lessonFlow.js           ← luồng 4 phần của 1 bài học
│   └── app.js                  ← sidebar, topbar, router
└── assets/
    ├── mascot/                ← ảnh nhân vật 鹿长老 (xem README.txt trong đó)
    └── audio/                  ← file mp3 phát âm thật (xem README.txt trong đó)
```

## 3. Cách thêm Bài 2 → Bài 14

Lộ trình 14 bài đã được đăng ký sẵn trong `js/data/course.js` (bài 2–14 đang
`locked: true, dataKey: null`, hiện "Sắp ra mắt"). Khi có tài liệu cho 1 bài:

1. Copy `js/data/lessons/lesson1.js` thành `lessonX.js`, giữ đúng 4 khối
   `warmup / vocabGrammar / explanation / practice`, thay nội dung theo bài mới,
   đổi biến thành `window.LESSON_X = {...}`.
2. Thêm `<script src="js/data/lessons/lessonX.js"></script>` vào `index.html`.
3. Trong `js/data/course.js`, sửa mục bài đó: `dataKey: "LESSON_X", locked: false`.
4. Trong `js/lib/state.js`, thêm `lessonX: { completedParts: {}, totalParts: 4,
   done: false, celebrated: false }` vào `lessonProgress`.

**Lưu ý quan trọng:** cấu trúc 4 phần hiện tại (热身/词汇&语法/讲解/练习 với đếm
ngược + ghi âm) được thiết kế cho dạng bài "khung nói/viết" như Bài 1. Hai dạng bài
đặc biệt trong lộ trình — **听后复述** (nghe rồi thuật lại) và **朗读** (đọc to) —
cần kiểu bài tập khác hẳn (nghe audio mẫu trước, đọc theo audio...). Khi làm tới các
bài đó, nên trao đổi trước để thiết kế lại phần `讲解`/`练习` cho đúng dạng kỹ năng,
thay vì áp nguyên khuôn hiện tại.

## 4. Thêm video hướng dẫn cho phần 讲解

Mỗi mục trong 讲解 (Dạng đề, Mở bài 1, Mở bài 2, Thân bài, Kết bài) có sẵn 1 khung
"🎬 Video hướng dẫn" ở cuối. Vì đây là site tĩnh (không có server lưu trữ), có 2
cách dùng:

- **Xem trước nhanh:** bấm "📤 Tải video lên" để chọn 1 file video từ máy — xem
  ngay lập tức, nhưng chỉ tồn tại trong phiên trình duyệt hiện tại (mất khi tải lại
  trang). Phù hợp khi bạn đang dạy trực tiếp từ máy của mình.
- **Lưu vĩnh viễn cho học sinh xem mọi lúc:** tải video lên YouTube (chọn chế độ
  **Không công khai/Unlisted**) hoặc Google Drive (bật chia sẻ "Bất kỳ ai có link"),
  dán link vào ô "hoặc dán link video..." trên trang, rồi bấm
  **"📋 Sao chép mã lưu vĩnh viễn"** — mã này dán vào `js/lib/videoMap.js` là xong,
  video sẽ hiện sẵn cho mọi học sinh mở trang.

## 5. Về việc lưu tiến độ (XP, % hoàn thành) và bản ghi âm

Phiên bản này lưu XP/tiến độ **tạm thời trong bộ nhớ trình duyệt** — tải lại trang
sẽ mất tiến độ. Bản ghi âm của học sinh cũng chỉ tồn tại trong phiên đó, nhưng có
nút **"⬇ Tải xuống"** để lưu file ghi âm về máy ngay sau khi ghi — nên nhắc học sinh
tải về nếu cần giữ lại. Khi bạn tự host website thật cho lớp học, có 2 hướng nâng
cấp (nên trao đổi trước để chọn đúng hướng theo quy mô sử dụng):

- Lưu tiến độ đơn giản: bật `localStorage` trong `js/lib/state.js`.
- Lưu bài ghi âm + tiến độ cho nhiều học sinh: cần một backend nhỏ (Firebase,
  Supabase...) để upload file ghi âm lên và lưu điểm theo tài khoản — đây là công
  việc bổ sung, chưa nằm trong bản v1 này.

## 6. Hệ thống màu & font (để giữ nhất quán khi mở rộng)

- Accent chính: xanh mạ `#5B8C3E`
- Điểm nhấn văn hoá: đỏ son `#B33A2E`, vàng kim `#C9982E`
- Nền: `#FBF9F4` (giấy ấm), chữ chính: `#2B2B28`
- Font chữ Hán: Noto Sans SC — Font tiếng Việt/UI: Inter
- Toàn bộ token màu nằm ở đầu file `css/style.css` (phần `:root`) — đổi ở đó sẽ
  áp dụng cho toàn site.

## 7. Đã kiểm thử

Đã test tự động (giả lập micro): cả 4 phần của Bài 1 — trả lời quiz, ghi âm đếm
ngược (đúng format 3 phút suy nghĩ / 2,5 phút trả lời + tín hiệu "tít") ở cả 热身
và 练习, lật flashcard, ghép nối, kéo-thả luận điểm, sắp xếp câu, trình xây câu trả
lời, tải/dán video hướng dẫn, chuyển tab tự do, tiến độ lên đúng 100% kèm hiệu ứng
confetti. Responsive ở 3 kích thước (desktop 1440px, tablet 820px, mobile 375px),
không có lỗi console.
