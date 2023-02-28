# [Eagle ERP](https://www.eagle-erp.com "Eagle ERP's Homepage") Install Script

This script is based on the install script from André Schenkels (https://github.com/ShaheenHossain/eagle_erp_install_script/edit/eagleerp1221c-scripts)
but goes a bit further and has been improved. This script will also give you the ability to define an xmlrpc_port in the .conf file that is generated under /etc/
This script can be safely used in a multi-Eagle ERP code base server because the default Eagle ERP port is changed BEFORE the Eagle ERP is started.

## Installation procedure

##### 1. Download the script:
```
sudo wget https://raw.githubusercontent.com/ShaheenHossain/installScript_01/niirwebsys1230/eagle-install-1230.sh

```
sudo chmod +x eagle-install-1230.sh
```

```
sudo ./eagle-install-1230.sh
```


try with virtual env:

sudo apt update
sudo apt install -y build-essential wget python3-dev python3-venv python3-wheel libfreetype6-dev libxml2-dev libzip-dev libldap2-dev libsasl2-dev python3-setuptools node-less libjpeg-dev zlib1g-dev libpq-dev libxslt1-dev libldap2-dev libtiff5-dev libjpeg8-dev libopenjp2-7-dev liblcms2-dev libwebp-dev libharfbuzz-dev libfribidi-dev libxcb1-dev

sudo useradd -m -d /eagle1230 -U -r -s /bin/bash eagle1230

sudo apt install postgresql

sudo su - postgres -c "createuser -s eagle1230"

sudo wget https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox_0.12.5-1.bionic_amd64.deb
sudo apt install ./wkhtmltox_0.12.5-1.bionic_amd64.deb

sudo su - eagle1221

git clone https://github.com/ShaheenHossain/eagle12c1.2 --branch master /eagle1221/eagle1211-server

cd /eagle1221

python3 -m venv eagle1221-venv

source eagle1230-venv/bin/activate

pip3 install wheel

sudo pip3 install -r https://github.com/ShaheenHossain/requirements_12/raw/master/requirements.txt

deactivate
mkdir /eagle1230/custom
mkdir /eagle1230/custom/addons

exit

sudo nano /etc/eagle1230-server.conf

[options]
; Database operations password:
admin_passwd = PASSWORD
db_host = False
db_port = False
db_user = eagle1230
xmlrpc_port = 8030
db_password = False
addons_path = /eagle1230/eagle-server/eagle/addons,/eagle1230/eagle-server/custom/addons

sudo nano /etc/systemd/system/eagle1230.service

[Unit]
Description=Eagle
Requires=postgresql.service
After=network.target postgresql.service

[Service]
Type=simple
SyslogIdentifier=eagle1230
PermissionsStartOnly=true
User=eagle1230
Group=eagle1230
ExecStart=/eagle1230/eagle1230-server/eagle1230-venv/bin/python3 /eagle1230/eagle1230-server/eagle-bin -c /etc/eagle1230-server.conf
StandardOutput=journal+console

[Install]
WantedBy=multi-user.target


sudo systemctl daemon-reload
sudo systemctl enable --now eagle1230
sudo systemctl status eagle1230
sudo journalctl -u eagle1230

http://localhost:8069
