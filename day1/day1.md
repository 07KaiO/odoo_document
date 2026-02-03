# Ngày 1: Setup & Hello Odoo
- Cài đặt Ubuntu/WSL2, Python, PostgreSQL
    - Chuẩn bị hệ thống
    ```
    sudo apt update && sudo apt upgrade -y
    ```
    - Cài các gói cần thiết
    ```
    sudo apt install -y \
    git python3 python3-pip python3-venv python3-dev \
    build-essential libpq-dev \
    libxml2-dev libxslt1-dev \
    libldap2-dev libsasl2-dev \
    libjpeg-dev libffi-dev \
    nodejs npm \
    postgresql
    ```
    - Cài PostgreSQL & tạo user cho Odoo
    Vào postgres
    ```
    sudo -u postgres psql
    ```
    - Tạo user
    ```
    CREATE ROLE odoo WITH LOGIN SUPERUSER PASSWORD 'odoo';
    \q
    ```
- one Source Code Odoo 18
    - Tạo thư mục Odoo
    ```
    cd ~
    mkdir odoo18
    cd odoo18
    ```
    - Clone source Odoo 18
    ```
    git clone https://www.github.com/odoo/odoo --depth 1 --branch 18.0
    ```
    - Tạo Python virtual environment
    ```
    cd ~/odoo18
    python3 -m venv venv
    source venv/bin/activate
    ```
    - Cài Python dependencies
    ```
    pip install --upgrade pip wheel
    pip install -r odoo/requirements.txt
    ```
    - Cài thư viện frontend (less, rtlcss)
    ```
    sudo npm install -g less less-plugin-clean-css rtlcss
    ```

- file cấu hình cho odoo.conf
```
[options]
admin_passwd = admin
db_host = False
db_port = False
db_user = odoo
db_password = odoo

addons_path = ~/odoo18/odoo/addons,~/odoo18/custom_addons

xmlrpc_port = 8069
logfile = ~/odoo18/odoo.log

```
- Chạy server local
```
cd ~/odoo18
source venv/bin/activate
python3 odoo/odoo-bin -c odoo.conf
```
- Truy cập
```
http://localhost:8069
```

-Create Database
-Master password: admin
-Database name: <name_database>
