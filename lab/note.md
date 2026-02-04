# Tích hợp máy chấm công dòng Ronal Jack FA 2000 vào trong odoo 18

## 🎯 Mục tiêu

- Kết nối máy chấm công Ronal Jack FA-2000
- Lấy log chấm công
- Đổ vào hr.attendance của Odoo 18

---

1️⃣ Chuẩn bị thư viện kết nối máy
Cài thư viện `pyzk`
```
source venv/bin/activate
pip install pyzk
```
Test nhanh ngoài Odoo:
```
from zk import ZK

zk = ZK('192.168.1.201', port=4370, timeout=5)
conn = zk.connect()
logs = conn.get_attendance()
print(logs[:5])
conn.disconnect()
```
---
2️⃣ Tạo module Odoo
```
cd addons
odoo-bin scaffold attendance_fa2000 custom_addons
```
---
3️⃣ Khai báo module
`__manifest__.py`
```
{
    "name": "Attendance FA-2000 Integration",
    "version": "1.0",
    "depends": ["hr_attendance", "hr"],
    "author": "You",
    "category": "Human Resources",
    "installable": True,
}
```
---
4️⃣ Model cấu hình máy chấm công
`models/attendance_device.py`
```
from odoo import models, fields

class AttendanceDevice(models.Model):
    _name = "attendance.device"
    _description = "Attendance Device"

    name = fields.Char(required=True)
    ip = fields.Char(required=True)
    port = fields.Integer(default=4370)
    timeout = fields.Integer(default=5)
    active = fields.Boolean(default=True)
```
---
5️⃣ Kết nối & lấy dữ liệu chấm công
`models/attendance_sync.py`
```
from odoo import models
from zk import ZK
from datetime import datetime
import pytz

class AttendanceDeviceSync(models.AbstractModel):
    _name = "attendance.device.sync"

    def sync_attendance(self, device):
        zk = ZK(
            device.ip,
            port=device.port,
            timeout=device.timeout,
            password=0,
            force_udp=False
        )

        conn = zk.connect()
        conn.disable_device()

        attendances = conn.get_attendance()
        tz = pytz.timezone(self.env.user.tz or "UTC")

        for att in attendances:
            # att.user_id = mã nhân viên trên máy
            employee = self.env["hr.employee"].search(
                [("barcode", "=", att.user_id)],
                limit=1
            )
            if not employee:
                continue

            punch_time = tz.localize(att.timestamp).astimezone(pytz.UTC)

            self.env["hr.attendance"].create({
                "employee_id": employee.id,
                "check_in": punch_time,
            })

        conn.enable_device()
        conn.disconnect()
```
---
6️⃣ Nút bấm đồng bộ trong Odoo
models/attendance_device.py
```
def action_sync(self):
    self.env["attendance.device.sync"].sync_attendance(self)
```
---
7️⃣ View giao diện
views/attendance_device_view.xml
```
<odoo>
  <record id="view_attendance_device_form" model="ir.ui.view">
    <field name="name">attendance.device.form</field>
    <field name="model">attendance.device</field>
    <field name="arch" type="xml">
      <form>
        <header>
          <button name="action_sync"
                  type="object"
                  string="Sync Attendance"
                  class="btn-primary"/>
        </header>
        <sheet>
          <group>
            <field name="name"/>
            <field name="ip"/>
            <field name="port"/>
            <field name="timeout"/>
            <field name="active"/>
          </group>
        </sheet>
      </form>
    </field>
  </record>
</odoo>
```
---
8️⃣ Map nhân viên Odoo ↔ Máy chấm công
Trong form hr.employee

Điền:
```
Barcode = ID nhân viên trên FA-2000
```
---
9️⃣ Cron tự động (rất nên có)
data/cron.xml
```
<odoo>
  <record id="cron_sync_attendance" model="ir.cron">
    <field name="name">Sync FA-2000 Attendance</field>
    <field name="model_id" ref="model_attendance_device"/>
    <field name="state">code</field>
    <field name="code">
      model.search([('active','=',True)]).action_sync()
    </field>
    <field name="interval_number">10</field>
    <field name="interval_type">minutes</field>
  </record>
</odoo>
```