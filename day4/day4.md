# Ngày 4: Models - Dữ liệu

## 📘 Nội dung

### Định nghĩa Model (_name)
Trong Odoo, **Model** là thành phần cốt lõi dùng để **định nghĩa dữ liệu và nghiệp vụ** của hệ thống.

Odoo sử dụng **ORM (Object Relational Mapping)** để ánh xạ giữa:
- **Class Python** ↔ **Bảng trong cơ sở dữ liệu (PostgreSQL)**

Mỗi model trong Odoo tương ứng với **một bảng dữ liệu** trong database.
module_name.model_name

Ví dụ:
```python
class LibraryBook(models.Model):
    _name = 'library.book'
    _description = 'Library Book'
```
### Kiểu dữ liệu (Char, Int, Date)
- `Char` – Chuỗi ký tự (tên, mô tả ngắn)
- `Integer` (Int) – Số nguyên
- `Date` – Ngày tháng
Ví dụ:
```
name = fields.Char(string='Title', required=True)
quantity = fields.Integer(string='Quantity')
publish_date = fields.Date(string='Publish Date')
```

### Quan hệ (Many2one, One2many)

- Many2one
    - Quan hệ nhiều–một
    - Dùng để liên kết model hiện tại với model khác

- One2many
    - Quan hệ một–nhiều
    - Thường đi kèm với `Many2one`

Ví dụ:
```
author_id = fields.Many2one('library.author', string='Author')
book_ids = fields.One2many('library.book', 'author_id', string='Books')
```