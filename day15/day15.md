# Ngày 15: Security

## Nội dung bài học

### 1️⃣ ir.model.access.csv — Quyền CƠ BẢN
🔹 Dùng để làm gì?

Cấp quyền CRUD cho model

Trả lời câu hỏi:
👉 User này có được đọc / ghi / tạo / xoá model không?

📌 KHÔNG có access → record rule cũng vô dụng

🔹 Cấu trúc file
```
id,name,model_id:id,group_id:id,perm_read,perm_write,perm_create,perm_unlink
```

🔹 Ví dụ với res.partner
```
access_res_partner_user,res.partner user,base.model_res_partner,base.group_user,1,1,1,0
```
| Cột                 | Ý nghĩa                 |
| ------------------- | ----------------------- |
| `model_res_partner` | model                   |
| `group_user`        | user nội bộ             |
| `1,1,1,0`           | đọc / ghi / tạo / ❌ xoá |

👉 Nếu thiếu dòng này → user thường không sửa được Contacts

🔥 Nguyên tắc nhớ nhanh

Access = có quyền CHẠM model không?

---

### 2️⃣ Record Rules — Quyền THEO DÒNG (Row-level)
🔹 Dùng để làm gì?

Giới hạn NHỮNG RECORD NÀO user được thấy / sửa

Trả lời câu hỏi:
👉 Có quyền rồi, nhưng được làm trên record nào?

📌 Record rule = domain filter

🔹 Ví dụ kinh điển

User chỉ được sửa Contact do mình tạo

📄 security/record_rule.xml

```
<odoo>
    <record id="partner_own_records_rule" model="ir.rule">
        <field name="name">Partner: only own records</field>
        <field name="model_id" ref="base.model_res_partner"/>
        <field name="groups" eval="[(4, ref('base.group_user'))]"/>

        <field name="domain_force">
            [('create_uid', '=', user.id)]
        </field>

        <field name="perm_read" eval="1"/>
        <field name="perm_write" eval="1"/>
        <field name="perm_create" eval="1"/>
        <field name="perm_unlink" eval="0"/>
    </record>
</odoo>
```
📌 domain_force bắt buộc áp dụng, user không override được.
🔥 Nguyên tắc nhớ nhanh

Record Rule = được CHẠM record nào?

---

### 3️⃣ So sánh nhanh

| Tiêu chí | ir.model.access      | Record Rule              |
| -------- | -------------------- | ------------------------ |
| Mức độ   | Model                | Record                   |
| Kiểu     | CRUD                 | Domain                   |
| Không có | Không thao tác được  | Thấy 0 record            |
| Ví dụ    | Có quyền sửa Partner | Chỉ sửa partner của mình |

---

### 4️⃣ Thứ tự Odoo kiểm tra quyền
```
1️⃣ ir.model.access.csv
        ↓
2️⃣ Record Rules
        ↓
3️⃣ Field-level (groups=...)
```

❌ Fail ở bước nào → stop luôn

---

## 🧪 Bài tập Lab
Tạo nhóm 'Thủ thư' (Full quyền) và 'Độc giả' (Chỉ xem). Cấu hình Record Rule để độc giả chỉ thấy sách đang có sẵn.