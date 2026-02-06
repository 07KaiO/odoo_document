# Ngày 6: Actions & Menus

---

## 1️⃣ Menu cha – Menu con là gì?

👉 Menu trong Odoo chỉ là cái nút để mở màn hình, nó không chứa logic, logic nằm ở action.
- Menu cha: chỉ để gom nhóm, thường không có action
- Menu con: có action, bấm vào là mở view
## Ví dụ cấu trúc
```
Thư viện (Menu cha)
 ├── Sách (Menu con)
 └── Tác giả (Menu con)
```
---
## 2️⃣ ir.actions.act_window là gì?
👉 `ir.actions.act_window` là Action mở cửa sổ (list, form, kanban, …)

Nó trả lời 3 câu hỏi:
- Mở model nào
- Mở view gì
- Mở theo chế độ nào
  
---

## 3️⃣ Ví dụ đầy đủ: Menu + Action
### 3.1 Action mở danh sách Sách
```
<record id="action_library_book" model="ir.actions.act_window">
    <field name="name">Danh sách sách</field>
    <field name="res_model">library.book</field>
    <field name="view_mode">tree,form</field>
</record>
```
📌 Giải thích:

- `res_model`: model được mở
- `view_mode`: thứ tự view khi mở (`tree` trước, click vào mở `form`)

### 3.2 Menu cha
```
<menuitem
    id="menu_library_root"
    name="Thư viện"
    sequence="10"/>
```
📌 Menu này không có action → chỉ làm cha
### 3.3 Menu con gắn action
```
<menuitem
    id="menu_library_book"
    name="Sách"
    parent="menu_library_root"
    action="action_library_book"
    sequence="1"/>
```
📌 Khi bấm Thư viện → Sách → Odoo gọi `action_library_book`
---
## 4️⃣ Luồng hoạt động
```
Menu con
   ↓
ir.actions.act_window
   ↓
res_model (library.book)
   ↓
tree view → form view
```
👉 Menu không mở view trực tiếp
👉 Menu gọi action
👉 Action quyết định mở cái gì

---
## Lab
### Vi du
```
<?xml version="1.0" encoding="UTF-8"?>
<odoo>

    <menuitem id="menu_library_root" name="Library"/>

    <!-- Menu cha -->
    <menuitem
        id="my_library_root"
        name="My Library"
        parent="menu_library_root"
        action="action_library_book"
    />

    <!-- Menu con -->
    <menuitem
        id="menu_library_book"
        name="Books"
        parent="my_library_root"
        action="action_library_book"
    />

    <menuitem
        id="menu_library_author"
        name="Authors"
        parent="my_library_root"
        action="action_library_author"
    />

    <menuitem
        id="menu_library_category"
        name="Categories"
        parent="my_library_root"
        action="action_library_category"
    />

</odoo>
```
### 🗂️ Cấu trúc Menu cần tạo
```
📚 My library   (Menu cha)
 ├── 📘 Book
 ├── ✍️ Author
 └── 🏷️ Category
```