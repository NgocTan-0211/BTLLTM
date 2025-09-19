<h2 align="center">
    <a href="https://dainam.edu.vn/vi/khoa-cong-nghe-thong-tin">
    🎓 Faculty of Information Technology (DaiNam University)
    </a>
</h2>
<h2 align="center">
   ỨNG DỤNG TRẮC NGHIỆM TRỰC TUYẾN
</h2>
<div align="center">
    <p align="center">
        <img src="docs/aiotlab_logo.png" alt="AIoTLab Logo" width="170"/>
        <img src="docs/fitdnu_logo.png" alt="AIoTLab Logo" width="180"/>
        <img src="docs/dnu_logo.png" alt="DaiNam University Logo" width="200"/>
    </p>

[![AIoTLab](https://img.shields.io/badge/AIoTLab-green?style=for-the-badge)](https://www.facebook.com/DNUAIoTLab)
[![Faculty of Information Technology](https://img.shields.io/badge/Faculty%20of%20Information%20Technology-blue?style=for-the-badge)](https://dainam.edu.vn/vi/khoa-cong-nghe-thong-tin)
[![DaiNam University](https://img.shields.io/badge/DaiNam%20University-orange?style=for-the-badge)](https://dainam.edu.vn)

</div>

## 📖 1. Giới thiệu
Ứng dụng **Trắc nghiệm trực tuyến** theo mô hình Client-Server được triển khai bằng **Java**.

- **Server**: thực thi trên Java, lắng nghe kết nối TCP (port mặc định trong mã là `12345`), quản lý dữ liệu quiz và người dùng qua SQLite (`quiz.db`). Server chịu trách nhiệm phát câu hỏi, nhận đáp án và trả kết quả (ví dụ: `ANSWER_CORRECT` / `ANSWER_WRONG`).
- **Client**: là ứng dụng desktop sử dụng **Java Swing**, cung cấp giao diện đăng nhập, lấy danh sách quiz, mở quiz và nộp đáp án. Client kết nối đến server qua `Socket` (mặc định `localhost:12345`).
- **Lưu trữ dữ liệu**: sử dụng **SQLite** (file `quiz.db`) — mã server tạo bảng và dữ liệu mặc định khi chưa có.

### ⚙️ Ghi chú quan trọng về code hiện tại
- Client giao tiếp với server bằng tin nhắn text đơn giản (mỗi dòng một lệnh). Ví dụ: `LOGIN user pass`, `GETQUIZZES`, `OPENQUIZ 1`, `SUBMITANSWER <quizId> <questionId> <optionIndex>`.
- Server dùng `PreparedStatement` cho hầu hết truy vấn, DB file `quiz.db` sẽ được tạo tự động khi server khởi động (nếu chưa tồn tại).

---

## 🔧 2. Ngôn ngữ lập trình sử dụng
- **Ngôn ngữ**: Java (JDK 11+ recommended)
- **Giao diện**: Java Swing (`javax.swing`)
- **Mạng**: Java Sockets (`ServerSocket`, `Socket`)
- **Cơ sở dữ liệu**: SQLite qua JDBC (`jdbc:sqlite:quiz.db`)
- **Đa luồng**: sử dụng luồng cho mỗi client (`Thread`) để xử lý kết nối đồng thời

### Các package/libraries chính (theo mã nguồn)
- `java.net.*` (Socket, ServerSocket)
- `java.io.*` (PrintWriter, BufferedReader, InputStreamReader)
- `java.sql.*` (Connection, DriverManager, PreparedStatement, ResultSet)
- `javax.swing.*`, `java.awt.*` (GUI)

---

## 🚀 3. Các chức năng, hình ảnh
### Server
- Lắng nghe kết nối TCP (mặc định port 12345).
- Tạo và khởi tạo cơ sở dữ liệu SQLite (`quiz.db`) nếu chưa có.
- Cung cấp các lệnh: `LOGIN`, `GETQUIZZES`, `OPENQUIZ`, `SUBMITANSWER`, ...
- So sánh đáp án và trả về kết quả cho client (ví dụ: `ANSWER_CORRECT` / `ANSWER_WRONG`).

### Client (UI - Java Swing)
- Form đăng nhập (Username + Password).
- Nút `Get Quizzes` để lấy danh sách quiz từ server.
- Nút `Open Quiz 1` (ví dụ trong mã mẫu) để mở quiz có id = 1.
- Hiển thị câu hỏi và các lựa chọn (radio buttons).
- Nút `Submit Answer` để gửi đáp án lên server và nhận thông báo chính xác/không.

> Hình ảnh giao diện và logo được lưu trong thư mục `docs/` (nếu có). Bạn có thể cập nhật `docs/` bằng ảnh chụp màn hình giao diện `QuizClientSwing`.

---

## 🚀 4. Các bước cài đặt
### Yêu cầu
- JDK 11 hoặc mới hơn
- Thư viện JDBC cho SQLite: `sqlite-jdbc-{version}.jar` (ví dụ `sqlite-jdbc-3.36.0.3.jar`).

> Lưu ý: mã server sử dụng URL JDBC `jdbc:sqlite:quiz.db`. Nếu bạn chưa có driver SQLite trên classpath, cần tải jar và đặt vào cùng thư mục hoặc thêm vào classpath khi biên dịch và chạy.

### Các bước (Linux / macOS)
1. Mở terminal, vào folder chứa mã (`QuizServer.java`, `QuizClientSwing.java`, ...)
2. Đặt `sqlite-jdbc-VERSION.jar` vào thư mục dự án.
3. Biên dịch:
```bash
javac -cp .:sqlite-jdbc-3.36.0.3.jar *.java
```
4. Chạy server:
```bash
java -cp .:sqlite-jdbc-3.36.0.3.jar QuizServer
```
Server sẽ tạo file `quiz.db` nếu chưa tồn tại và lắng nghe trên port 12345.

5. Mở client (một cửa sổ khác):
```bash
java -cp .:sqlite-jdbc-3.36.0.3.jar QuizClientSwing
```
Client sẽ kết nối tới `localhost:12345` (theo mã nguồn). Ấn `Get Quizzes` để lấy danh sách, `Open Quiz 1` để mở, chọn đáp án và `Submit Answer` để nộp.

### Trên Windows (Command Prompt / PowerShell)
Biên dịch và chạy giống như trên, nhưng thay `:` bằng `;` cho classpath:
```powershell
javac -cp .;sqlite-jdbc-3.36.0.3.jar *.java
java -cp .;sqlite-jdbc-3.36.0.3.jar QuizServer
# rồi chạy client
java -cp .;sqlite-jdbc-3.36.0.3.jar QuizClientSwing
```

### Khắc phục sự cố thường gặp
- **Ấn đăng nhập bị treo / không phản hồi**: kiểm tra chắc server đã chạy; firewall có thể chặn port 12345; client hiện tại dùng kết nối đồng bộ, nếu server không phản hồi sẽ thấy delay.
- **Không tìm thấy driver SQLite**: đảm bảo `sqlite-jdbc` jar đã được thêm đúng vào classpath khi biên dịch và chạy.
- **DB file `quiz.db`**: nếu muốn reset dữ liệu, xóa file `quiz.db` rồi khởi động lại server để tạo lại bảng mẫu.

---

## 📝 License

Họ và tên: Lù Ngọc Tân
Mã SV: 1671020281
Số điện thoại: 0906165171
Email: tanbonben@gmail.com
Lớp: CNTT 16-01

---

