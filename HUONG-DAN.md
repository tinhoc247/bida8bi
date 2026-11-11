# Bida Vui 🎱 (game độc lập)
Chỉ cần 1 file `index.html` – đưa lên Netlify / GitHub Pages / mở trực tiếp đều được. Không phụ thuộc game hub.

- **Chơi với máy**: dùng được ngay, không cần mạng hay Firebase.
- **Online 2 người**: cần Firebase Realtime Database + Anonymous Auth.
  1. Trong `index.html` đổi 2 hằng số `K` (apiKey) và `DB` (databaseURL) sang dự án Firebase của riêng bạn (tìm dòng `const K=`).
  2. Rules:
```json
{ "rules": { "bida": { "rooms": { "$code": { ".read": "auth != null", ".write": "auth != null" } } } } }
```
Tên game: sửa trong `<title>` và thẻ `<h1>`.

## Danh sách người chơi online + lời mời (mới)
- Tên chỉ nhập **một lần** (lưu trong trình duyệt), sau đó hiện "Xin chào, <tên>" và nút **Đổi tên**.
- Ở chế độ **2 người** có danh sách **🟢 Người chơi đang online**, có ô **Tìm theo tên**. Bấm **Mời** → tự tạo phòng và gửi lời mời; bên kia thấy hộp **Đồng ý / Từ chối**.
- Cần thêm rule Firebase này **bên trong** `"bida": { ... }` (cạnh `"rooms"`), rồi bấm Publish:
"online": {
  "$g": {
    ".read": "auth != null",
    "$uid": {
      ".write": "auth != null && auth.uid === $uid",
      "n": { ".validate": "newData.isString() && newData.val().length <= 40" },
      "t": { ".validate": "newData.isNumber()" },
      "s": { ".validate": "newData.isNumber()" },
      "inv": {
        ".write": "auth != null",
        ".validate": "newData.hasChildren(['f','n','r','t']) && newData.child('n').isString() && newData.child('n').val().length <= 40 && newData.child('r').isString() && newData.child('r').val().length <= 10"
      },
      "$other": { ".validate": false }
    }
  }
}
