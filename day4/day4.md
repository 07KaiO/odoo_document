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

### Quan hệ (Many2one, One2many)