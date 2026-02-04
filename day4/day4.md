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

#### 🔗 Quan hệ giữa các Model trong Odoo

Trong Odoo, quan hệ giữa các model được dùng để **liên kết dữ liệu** và **mô hình hóa nghiệp vụ thực tế**.

---

##### 🔹 Many2one (Nhiều – Một)

- Thể hiện quan hệ **nhiều bản ghi thuộc về một bản ghi khác**
- Dùng để liên kết model hiện tại với **một model khác**
- Thường được hiển thị dưới dạng **dropdown** trên giao diện

Ví dụ:
```python
author_id = fields.Many2one(
    'library.author',
    string='Author'
)
```
📌 Ý nghĩa:
- Mỗi Book chỉ có một Author
- Một Author có thể có nhiều Book

##### 🔹 One2many (Một – Nhiều)

- Thể hiện quan hệ một bản ghi liên kết với nhiều bản ghi khác

- Luôn đi kèm với một field Many2one ở model kia

- Không tạo cột mới trong database (dựa vào Many2one)

Ví dụ:
```
book_ids = fields.One2many(
    'library.book',
    'author_id',
    string='Books'
)
```

📌 Ý nghĩa:
- Một Author có nhiều Book
- Quan hệ được xác định thông qua field author_id


CẤU TRÚC THƯ MỤC MODULE `my_library`

```
my_library/
├── __init__.py
├── __manifest__.py
│
├── models/
│   ├── __init__.py
│   ├── book.py
│   ├── author.py
│   └── category.py
│
├── views/
│   ├── book_views.xml
│   ├── author_views.xml
│   ├── category_views.xml
│   └── book_menu.xml
│
├── security/
│   ├── ir.model.access.csv
│
├── data/
│   ├── author_demo.xml
│   ├── category_demo.xml
│   └── book_data.xml
│
├── controllers/
    ├── __init__.py
    └── main.py

```

1. `__init__.py` (ROOT)
```
from . import controllers
from . import models

```
2. `models/__init__.py`
```
from . import book
from . import author
from . import category
```
3. `__manifest__.py`
```
{
    'name': 'My Library',
    'version': '1.0',
    'category': 'Education',
    'summary': 'Simple Library Management',
    'description': 'Manage books with auto sample data',
    'author': 'KaiO',
    'depends': ['base'],
    'data': [
        'security/ir.model.access.csv',

        'views/book_views.xml',
        'views/book_actions.xml',
        'views/author_views.xml',
        'views/category_views.xml',
        'views/book_menu.xml',

        
        'data/category_data.xml',
        'data/author_data.xml',
        'data/book_data.xml'
    ],
    'installable': True,
    'application': True,
}

```
4. `security/ir.model.access.csv`
```
id,name,model_id:id,group_id:id,perm_read,perm_write,perm_create,perm_unlink

access_library_book,access_library_book,model_library_book,,1,1,1,1
access_library_author,access_library_author,model_library_author,,1,1,1,1
access_library_category,access_library_category,model_library_category,,1,1,1,1
```


## Ket qua
![ket qua](image/result.png)