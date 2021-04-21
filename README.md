# RPI
on rpi
## script tự động backup ##

Đoạn script sau đây (sử dụng Shell Script) sẽ thực hiện công việc tự động backup Source Code và Database (MySQL/MariaDB) hàng ngày trên Linux Server sử dụng Cron Job.
Tạo 1 script backup có đường dẫn: `/opt/scripts/backup.sh`
```
#! /bin/sh
# get today in format Ymd
TODAY=$(date +"%Y%m%d")
DB_NAME=<database_name>
DB_USER=<database_user>
SITE='vinasupport'
# command for backup
cd /opt/backup
mysqldump -u ${DB_USER} --single-transaction --quick --lock-tables=false ${DB_NAME} | gzip  > ${DB_NAME}-${TODAY}.sql.gz
zip -r ${SITE}-${TODAY}.zip /opt/www/vinasupport/ > /dev/null 2>&1
```

Với:
  /opt/backup là thư mục lưu trữ các bản backup
  /opt/www/vinasupport là thư mục lưu trữ source code của website
  <database_name> là tên của CSDL
  <database_user> là tên của User của MySQL/MariaDB Database

Phân quyền cho script backup
`sudo chmod +x /opt/scripts/backup.sh`

Đoạn script ở trên ở lệnh mysqldump không sử dụng tham số -p để tránh đặt password trong script và cảnh bảo khi thực hiện lệnh mysqldump. Thay vào đó, setting username + password vào file config my.conf của MySQL/MariaDB.
Mở file /etc/my.conf, tìm đển đoạn [mysqldump] và thêm các tham số user và password của MySQL/MariaDB vào sau:
```
[mysqldump]
user=<database_user>
password=<password>
```
Restart lại dịch vụ MySQL/MariaDB để nhận config mới.
Thiết lập Cron Job để tự động backup hàng ngày

Thực hiện command crontab -e trên server để thiết lập 1 job tự động backup hàng ngày
```
# ┌───────────── minute (0 - 59) 
# │ ┌───────────── hour (0 - 23) 
# │ │ ┌───────────── day of the month (1 - 31) 
# │ │ │ ┌───────────── month (1 - 12) 
# │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday; 
# │ │ │ │ │                                   7 is also Sunday on some systems) 
# │ │ │ │ │ 
# │ │ │ │ │ 
# * * * * * <command to execute>
```
```
# m h  dom mon dow   command
0 0 * * * /opt/scripts/backup.sh >/dev/null 2>&1
```
Đoạn script sẽ tự động được chạy bởi Cron Job lúc 00:00 hàng ngày theo múi giờ thiết lập trên server.
![image](https://user-images.githubusercontent.com/21152088/115555793-21e29600-a2da-11eb-8753-2fc8f7e7fb62.png)

