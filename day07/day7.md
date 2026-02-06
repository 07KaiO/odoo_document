# Ngày 7: Tree & Form View

## 📘 Nội dung học tập
### 1️⃣ List view (Tree view) – Structure
List view = màn hình danh sách record.
Cấu trúc cơ bản
```
<tree string="Danh sách sách">
    <field name="name"/>
    <field name="author_id"/>
    <field name="category_id"/>
</tree>
```
Những thành phần quan trọng
1. `<tree>`

Đại diện list view

`string`: tiêu đề

2. `<field>`

Cột trong bảng

Thứ tự field = thứ tự cột

---
Thuộc tính hay dùng trong List view
```
<tree editable="bottom" create="true" delete="true">
```
| Thuộc tính          | Ý nghĩa                  |
| ------------------- | ------------------------ |
| `editable="bottom"` | Thêm/sửa nhanh dưới list |
| `editable="top"`    | Thêm/sửa trên đầu        |
| `create="false"`    | Ẩn nút Create            |
| `delete="false"`    | Không cho xóa            |
| `default_order`     | Thứ tự sắp xếp           |

---
Ví dụ List view đầy đủ
```
<tree string="Sách" default_order="name asc">
    <field name="name"/>
    <field name="author_id"/>
    <field name="category_id"/>
    <field name="publish_date"/>
</tree>
```
---
### 2️⃣ Form view – Layout chuẩn

Form view = màn hình xem / chỉnh sửa chi tiết

Odoo layout theo từng tầng rất rõ ràng 👇
```
form
 └─ sheet
     ├─ group
     │   └─ field
     └─ notebook
         └─ page
```
---
### 3️⃣ <sheet> – khung chính 📄
```
<form string="Sách">
    <sheet>
        ...
    </sheet>
</form>
```
📌 Nếu không có <sheet> → form nhìn rất xấu
📌 <sheet> tạo nền trắng, căn giữa
---
### 4️⃣ <group> – chia cột thông minh
```
<group>
    <field name="name"/>
    <field name="author_id"/>
</group>
```
📌 Mặc định:

1 <group> = 2 cột

Field tự căn đều

Group lồng group
```
<group>
    <group>
        <field name="name"/>
        <field name="author_id"/>
    </group>
    <group>
        <field name="category_id"/>
        <field name="publish_date"/>
    </group>
</group>
```
---
### 5️⃣ <notebook> – Tabs
Dùng khi form nhiều thông tin
```
<notebook>
    <page string="Thông tin">
        <group>
            <field name="description"/>
        </group>
    </page>

    <page string="Khác">
        <group>
            <field name="note"/>
        </group>
    </page>
</notebook>
```
📌 Mỗi <page> = 1 tab

---

### 6️⃣ Ví dụ Form view hoàn chỉnh (Library Book)
```
<form string="Sách">
    <sheet>

        <group>
            <group>
                <field name="name"/>
                <field name="author_id"/>
            </group>
            <group>
                <field name="category_id"/>
                <field name="publish_date"/>
            </group>
        </group>

        <notebook>
            <page string="Mô tả">
                <field name="description"/>
            </page>
            <page string="Ghi chú">
                <field name="note"/>
            </page>
        </notebook>

    </sheet>
</form>
```