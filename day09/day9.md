# Ngày 9: Data Files

## 📘 Nội dung học tập
### 1️⃣ Import Demo Data trong Odoo là gì?

👉 Demo data = dữ liệu mẫu để:
- Test module
- Demo cho khách
- Học nghiệp vụ

📌 Demo data chỉ được load khi:
- Cài module có tick “Load demo data”
- HOẶC chạy với `--load-demo`

---

### 2️⃣ Cấu trúc file demo data chuẩn
Thông thường:
```
my_library/
├── data/          # data thật
├── demo/          # demo data
│   ├── book_demo.xml
│   └── book_demo.csv
```
---

### 3️⃣ Khai báo demo data trong __manifest__.py
```
{
    'name': 'My Library',
    'data': [
        'views/book_views.xml',
    ],
    'demo': [
        'demo/book_demo.xml',
        # hoặc 'demo/book_demo.csv'
    ],
}
```
📌 File trong demo:
- ❌ Không load khi update module
- ✅ Chỉ load khi cài mới + tick demo
  
---

### 4️⃣ Import Demo Data bằng XML
Ví dụ: Demo tác giả
```
<odoo>
    <record id="author_demo_rowling" model="library.author">
        <field name="name">J.K. Rowling</field>
    </record>

    <record id="author_demo_tolstoy" model="library.author">
        <field name="name">Leo Tolstoy</field>
    </record>
</odoo>
```
📌 Ưu điểm:
- Dễ gán Many2one / Many2many
- Dễ đọc
- Dùng được ref()

---

### 5️⃣ Import Demo Data bằng CSV
Ví dụ: Demo sách
```
id,name,code,author_id/id
book_hp,Harry Potter,HP001,author_demo_rowling
book_war,War and Peace,WP001,author_demo_tolstoy
```
📌 Quy tắc CSV:
- Dòng đầu = tên field
- Many2one: `field/id`
- Many2many: `field/id` (nhiều id, cách nhau bằng `,`)

---

### 6️⃣ So sánh XML vs CSV 🧠

| Tiêu chí       | XML | CSV |
| -------------- | --- | --- |
| Dễ đọc         | ⭐⭐⭐ | ⭐   |
| Quan hệ        | ⭐⭐⭐ | ⭐⭐  |
| Dữ liệu lớn    | ⭐   | ⭐⭐⭐ |
| Logic phức tạp | ⭐⭐⭐ | ❌   |


👉 Ít record → XML
👉 Nhiều record → CSV

---

### 7️⃣ noupdate="1" là gì?

```
<odoo noupdate="1">
    <record id="author_admin" model="library.author">
        <field name="name">Admin Author</field>
    </record>
</odoo>
```
👉 Khi:
- Update module (-u)
- Odoo KHÔNG update record này nữa

📌 Nghĩa là:
- Cài lần đầu → tạo record
- Update module → GIỮ NGUYÊN DATA

---

### 8️⃣ Khi nào PHẢI dùng noupdate="1"?
| Trường hợp           | Có dùng?        |
| -------------------- | --------------- |
| Demo data            | ❌ (để sửa được) |
| Dữ liệu cấu hình     | ✅               |
| Menu, action         | ❌               |
| Master data hệ thống | ✅               |


---

## 🧪Bài tập Lab
Viết file XML để tự động tạo 10 cuốn sách mẫu ngay khi cài module.
- code
data/category.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<odoo>
    <data noupdate="0">

        <record id="cat_programming" model="library.category">
            <field name="name">Programming</field>
        </record>

        <record id="cat_software" model="library.category">
            <field name="name">Software Engineering</field>
        </record>

        <record id="cat_architecture" model="library.category">
            <field name="name">Software Architecture</field>
        </record>

    </data>
</odoo>
```
data/book_demo.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<odoo>

    <!-- 10 cuốn sách demo -->
    <record id="book_demo_01" model="library.book">
        <field name="name">Book Demo 01</field>
        <field name="code">B001</field>
        <field name="author_id" ref="author_bloch"/>
    </record>

    <record id="book_demo_02" model="library.book">
        <field name="name">Book Demo 02</field>
        <field name="code">B002</field>
        <field name="author_id" ref="author_bloch"/>
    </record>

    <record id="book_demo_03" model="library.book">
        <field name="name">Book Demo 03</field>
        <field name="code">B003</field>
        <field name="author_id" ref="author_fowler"/>
    </record>

    <record id="book_demo_04" model="library.book">
        <field name="name">Book Demo 04</field>
        <field name="code">B004</field>
        <field name="author_id" ref="author_evans"/>
    </record>

    <record id="book_demo_05" model="library.book">
        <field name="name">Book Demo 05</field>
        <field name="code">B005</field>
        <field name="author_id" ref="author_evans"/>
    </record>

    <record id="book_demo_06" model="library.book">
        <field name="name">Book Demo 06</field>
        <field name="code">B006</field>
        <field name="author_id" ref="author_evans"/>
    </record>

    <record id="book_demo_07" model="library.book">
        <field name="name">Book Demo 07</field>
        <field name="code">B007</field>
        <field name="author_id" ref="author_evans"/>
    </record>

    <record id="book_demo_08" model="library.book">
        <field name="name">Book Demo 08</field>
        <field name="code">B008</field>
        <field name="author_id" ref="author_uncle_bob"/>
    </record>

    <record id="book_demo_09" model="library.book">
        <field name="name">Book Demo 09</field>
        <field name="code">B009</field>
        <field name="author_id" ref="author_uncle_bob"/>
    </record>

    <record id="book_demo_10" model="library.book">
        <field name="name">Book Demo 10</field>
        <field name="code">B010</field>
        <field name="author_id" ref="author_uncle_bob"/>
    </record>

</odoo>
```
- Result
![result](image/day9.png)