# Hướng dẫn thêm đáp án

### 1. Clone repo về máy và chạy extension ở môi trường dev

1. `git clone https://github.com/Pear104/coursera-tool.git`
2. `npm i`
3. `npm run dev`
4. Vào link [này](chrome://extensions/) và thêm folder `build-development` để chạy extension môi trường dev
5. Xóa hoặc tắt extension cũ trong máy (Nếu có)

### 2. Vào folder chứa đáp án và tạo file json

- Vào folder src/contentScript/keys/
- Tạo file json chứa đáp án để test (tên file là tên môn). Ví dụ: `src/contentScript/keys/SSL101c.json`
- File JSON sẽ có dạng như sau:

```json
{
  "quizSrc": [
    {
      "term": "Câu hỏi 1 | Đáp án 1 | Đáp án 2 | Đáp án 3 | Đáp án 4",
      "definition": "Đáp án 1"
    },
    {
      "term": "Câu hỏi 2 | Đáp án 1 | Đáp án 2 | Đáp án 3 | Đáp án 4",
      "definition": "Đáp án 2 | Đáp án 3" // Đối với câu hỏi có nhiều đáp án
    }
  ]
}
```

- Vào file `src/contentScript/api-services.ts` và đổi import đáp án test thành file JSON vừa tạo

```ts
// src/contentScript/api-services.ts dòng số 6
import testSource from "./keys/SSL101c.json";
// Đổi DXE291c.json thành tên file json vừa tạo
```

- Comment dòng 33 và bỏ comment dòng 34

```ts
// choosenSource = course.quizSrc;
choosenSource = testSource.quizSrc;
```

### 3. Tạo đáp án

#### Cách 1: Thêm đáp án có sẵn từ Quizlet (Không khuyến khích)

- Tải [extension](https://chromewebstore.google.com/detail/quizlet-getter-get-quizle/oaaodhahcdoonllnnhamgkcidjicjcad) lấy đáp án Quizlet và tải đáp án theo format JSON
- Thêm đáp án vừa tải vào file
- Sau mỗi lần thêm đáp án mới, vào file `vite.config.ts` CTRL+S để reload

> Cách này không khuyến khích vì
>
> - Đáp án có thể không chuẩn
> - Đáp án có thể không khớp với text extract được từ Coursera

#### Cách 2: Thêm đáp án thủ công (Hơi cực nhưng khuyến khích)

- Thêm Gemini API key
- Mở devtool và chạy Quiz (Nhớ tắt auto-submit)
- Copy response trả về từ Gemini:

  ![](/images/Screenshot%202025-05-31%20185641.png)

- Thêm đáp án vừa copy vào file (Nhớ bỏ cặp dấu "[]" của mảng đi)
- Sau mỗi lần thêm đáp án mới, vào file `vite.config.ts` CTRL+S để reload

### 4. Thêm đáp án vào source chính

- Nếu sau khi đã test và thấy đáp án của bạn hoạt động tốt, ngon nghẻ, và muốn đóng góp vào source chính cho mọi người xài chung thì hãy:

1. Clone repo `fuquiz-db` này
2. Tạo branch riêng có tên của môn đó
3. Thêm file đáp án JSON vừa tạo
4. Vào file `courseMap.json` điều chỉnh một số thông tin về môn

Ví dụ:

```json
{
  "DXE291c": {
    "status": "done",
    "related": [
      // Url đến các MOOC của môn đó
      "/qun-l-nh-nc-v-hnh-chnh-cng",
      "/dxe291-3",
      "/kinh-t-s-v-ti-chnh-s",
      "/tri-nghim-khch-hng-v-tip-th-s"
    ]
  }
}
```

Lấy các link related kiểu gì?

- Lấy đường dẫn nhỏ sau `coursera.org/learn/` và thêm vào field `related` của môn đó

![](/images/Screenshot%202025-05-31%20201204.png)

5. Tạo pull request

Inbox [mình](https://www.facebook.com/truong.le.567651) nếu cần hồ trợ

Mình code khá gà, dự án ban đầu tính làm để mình đọc thôi, mong các bạn đừng chửi 😅, nếu sau này nhiều người contribute thì sẽ xem xét refactor lại 😥
