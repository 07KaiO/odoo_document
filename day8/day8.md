# Ngày 8: Search & Filters

## 📘 Nội dung học tập
### 1️⃣ Search View là gì?

👉 Search view = thanh tìm kiếm + bộ lọc + group by phía trên list view.

Nó trả lời:
- Tìm theo field nào
- Có những filter sẵn nào
- Cho group by theo tiêu chí gì
  
---

### 2️⃣ Cấu trúc Search View cơ bản
```
<search string="Tìm kiếm">
    <field name="name"/>
    <filter string="..." domain="[...]"/>
    <group>
        <filter context="{'group_by':'...'}"/>
    </group>
</search>
```

---

### 3️⃣ Filter & Group By 
🎯 Ví dụ: Library – Book
#### 3.1 Filter theo điều kiện (nút bấm)
```
<search string="Tìm sách">

    <!-- Search nhanh -->
    <field name="name"/>
    <field name="author_id"/>

    <!-- Filter -->
    <filter
        string="Còn sách"
        name="available"
        domain="[('is_available','=',True)]"/>

    <filter
        string="Hết sách"
        name="not_available"
        domain="[('is_available','=',False)]"/>

    <!-- Group By -->
    <group expand="0" string="Nhóm theo">
        <filter
            string="Tác giả"
            context="{'group_by':'author_id'}"/>
        <filter
            string="Thể loại"
            context="{'group_by':'category_id'}"/>
    </group>

</search>
```
📌 Kết quả:
- Có nút Còn sách / Hết sách
- Group By theo Tác giả / Thể loại

---

### 4️⃣ Filter động (chọn giá trị)
Ví dụ: “Sách của tác giả X”
```
<search string="Tìm sách">

    <!-- Filter động -->
    <field name="author_id" string="Tác giả"/>
    <field name="category_id" string="Thể loại"/>

</search>
```
📌 Khi user chọn:

Author = X

→ Domain tự sinh:
```
[('author_id', '=', X)]
```
👉 Đây là cách hay nhất cho filter theo Many2one

---

### 5️⃣ Searchpanel là gì?
👉 Searchpanel = panel bên trái (giống Odoo Sales, Inventory)

Dùng để:

Click nhanh
- Lọc theo 1 – 2 field chính
- UX rất tốt

---

### 6️⃣ Cấu trúc Searchpanel
```
<search>
    <searchpanel>
        <field name="category_id"/>
        <field name="author_id"/>
    </searchpanel>
</search>
```
📌 Hiện panel bên trái với:
- Danh sách Thể loại
- Danh sách Tác giả

---

### 7️⃣ Searchpanel nâng cao
Ví dụ: Category → Author
```
<search string="Tìm sách">

    <searchpanel>
        <field
            name="category_id"
            icon="fa-tags"/>

        <field
            name="author_id"
            icon="fa-user"
            enable_counters="1"/>
    </searchpanel>

</search>
```
📌 `enable_counters`
→ hiện số sách trong mỗi nhóm

---

### 8️⃣ Kết hợp Search View + Searchpanel
```
<search string="Tìm sách">

    <!-- Search -->
    <field name="name"/>

    <!-- Filter -->
    <filter
        string="Còn sách"
        domain="[('is_available','=',True)]"/>

    <!-- Group By -->
    <group expand="0">
        <filter
            string="Thể loại"
            context="{'group_by':'category_id'}"/>
    </group>

    <!-- Searchpanel -->
    <searchpanel>
        <field name="category_id"/>
        <field name="author_id"/>
    </searchpanel>

</search>
```

---

### 9️⃣ Gắn Search View vào Action 

```
<record id="action_library_book" model="ir.actions.act_window">
    <field name="name">Sách</field>
    <field name="res_model">library.book</field>
    <field name="view_mode">tree,form</field>
    <field name="search_view_id" ref="view_library_book_search"/>
</record>
```

## Lab
![result](image/result.png)