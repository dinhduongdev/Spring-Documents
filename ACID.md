
# Giao tác và Tính chất ACID trong Cơ sở dữ liệu

## 1. Giao tác là gì?

**Giao tác (Transaction)** là một nhóm các thao tác (thêm, sửa, xóa dữ liệu) được thực hiện như một khối duy nhất.

**Ví dụ:** Chuyển tiền từ tài khoản A sang tài khoản B:
- Trừ tiền tài khoản A
- Cộng tiền tài khoản B
=> Nếu 1 bước lỗi, tất cả phải hủy. Đó là giao tác.

---

## 2. Autocommit trong JDBC

Mặc định JDBC **tự động commit** mỗi câu lệnh SQL.

Điều này nghĩa là: mỗi `INSERT`, `UPDATE`, hay `DELETE` sẽ **lưu ngay vào CSDL**.

Muốn dùng giao tác thực sự (để rollback khi lỗi), cần **tắt autocommit**:

```java
conn.setAutoCommit(false);
```

---

## 3. Ví dụ giao tác trong Java

```java
Connection conn = DriverManager.getConnection(...);
conn.setAutoCommit(false);  // Tắt autocommit

try {
    Statement stmt = conn.createStatement();
    stmt.executeUpdate("UPDATE account SET balance = balance - 100 WHERE id = 1");
    stmt.executeUpdate("UPDATE account SET balance = balance + 100 WHERE id = 2");

    conn.commit(); // Lưu thay đổi nếu thành công
} catch (Exception e) {
    conn.rollback(); // Hủy nếu có lỗi
    e.printStackTrace();
}
```

---

## 4. Tính chất ACID

| Tính chất | Giải thích |
|----------|------------|
| **A - Atomicity (Nguyên tố)** | Giao tác là tất cả hoặc không gì cả. Nếu 1 bước lỗi → hủy toàn bộ |
| **C - Consistency (Nhất quán)** | CSDL luôn trong trạng thái hợp lệ, trước và sau giao tác |
| **I - Isolation (Cô lập)** | Giao tác đồng thời không ảnh hưởng nhau |
| **D - Durability (Bền vững)** | Dữ liệu sau commit sẽ tồn tại vĩnh viễn |

---

## 5. Kết luận

> Giao tác đảm bảo **an toàn dữ liệu** trong các hoạt động quan trọng như chuyển tiền, cập nhật đơn hàng,...  
> Nhờ ACID, dữ liệu không bị sai, không bị mất, và không bị lẫn khi nhiều người cùng thao tác.



# Batch Query trong JDBC

## 1. Khái niệm

**Batch Query** là kỹ thuật gom nhiều câu lệnh SQL lại thành một lô (batch) và thực hiện cùng lúc để:
- Giảm số lần tương tác với cơ sở dữ liệu
- Cải thiện hiệu năng chương trình

**Lưu ý:** Cần kiểm tra DBMS có hỗ trợ không bằng:

```java
DatabaseMetaData metadata = conn.getMetaData();
boolean supportsBatch = metadata.supportsBatchUpdates();
```

---

## 2. Các phương thức liên quan

| Phương thức | Mô tả |
|-------------|------|
| `addBatch()` | Thêm câu SQL vào batch |
| `executeBatch()` | Thực thi toàn bộ các câu SQL trong batch, trả về mảng `int[]` đại diện số dòng bị ảnh hưởng |
| `clearBatch()` | Xóa tất cả câu SQL đã thêm vào batch |

---

## 3. Ví dụ sử dụng Batch Query

```java
DatabaseMetaData metadata = conn.getMetaData();

if (metadata.supportsBatchUpdates()) {
    conn.setAutoCommit(false);  // Tắt autocommit để xử lý giao tác

    Statement stm = conn.createStatement();

    // Thêm câu lệnh vào batch
    stm.addBatch(String.format(query, "AUTHOR0025", "Lộc", "Trần"));
    stm.addBatch(String.format(query, "AUTHOR0026", "Phúc", "Hồ"));

    // Thực thi batch
    int[] kq = stm.executeBatch();

    // Commit toàn bộ thay đổi
    conn.commit();
}
```

---

## 4. Kết luận

> Batch Query giúp thực thi nhiều câu lệnh SQL hiệu quả hơn, tránh việc kết nối đi về giữa ứng dụng và cơ sở dữ liệu nhiều lần.
