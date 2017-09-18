#!/usr/bin/python
# coding:utf-8

# python lib
import psutil
import time
import smtplib
from email.mime.text import MIMEText

# owner global values
# 监控流量
# 服务器网速，单位为MBPS

# 发送邮件
def send_email(monitor_type, content,address):
    sender = '@eefung.com'
    receiver = '@eefung.com'
    subject = 'Server Warning'
    smtpserver = 'mail.eefung.com'
    username = '@eefung.com'
    password = ''

    email_content = 'Warning: ' + monitor_type[0] + '    Info: ' + monitor_type[1]
    for info in content:
        detail = ''
        for (key, value) in info.items():
            detail = detail + str(key) + ":" + str(value) + '   '
        email_content = email_content + ' ' + detail

    email_content = '' + email_content + '' + address

    msg = MIMEText(email_content, 'html', 'utf-8')

    msg['Subject'] = subject
    smtp = smtplib.SMTP()
    smtp.connect(smtpserver,'25')
    smtp.login(username, password)
    smtp.sendmail(sender, receiver, msg.as_string())
    smtp.quit()

# 获取硬盘使用信息
def disk_info():
    disk_info_list = []
    disk_info_dict = {}
    disk_all_mount_info = psutil.disk_partitions()
    
    b = []
    for i in disk_all_mount_info:
        b.append(i.mountpoint)

    for a in b:
        disk = psutil.disk_usage(a)
        # 硬盘总量
        disk_info_dict["total"] = disk.total
        # 硬盘使用量
        disk_info_dict["used"] = disk.used
        # 硬盘剩余量
        disk_info_dict["free"] = disk.free
        # 硬盘使用比
        disk_info_dict["percent"] = disk.percent

        disk_info_list.append(disk_info_dict)

        return disk_info_list

# 优雅获取本机IP地址
def ip_address():
    ip_info = psutil.net_if_addrs()['eth0'][0].address
    return ip_info 

# 监控磁盘使用
def monitor_disk():
    disk_all_mount_info = psutil.disk_partitions()

    b = []
    for i in disk_all_mount_info:
        if i != '/boot' and i != '/swap': 
            b.append(i.mountpoint)

    for a in b:
        disk_percent = psutil.disk_usage(a).percent
        # 磁盘使用率
        if disk_percent > 80:
            disk_info_list = disk_info()
            ip_info_address = ip_address()
            send_email(["disk", str(disk_percent)+'%'], disk_info_list, ip_info_address)


def monitor():
#     while 1:
     monitor_disk()
         # 间隔2秒避免提高cpu
#         time.sleep(2)


def main():
    monitor()


if __name__ == "__main__":
    main()
