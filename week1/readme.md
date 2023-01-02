## 安裝VM虛擬機
三台,每台CPU要至少兩個核心,2G的RAM,一張NAT網路卡
## 安裝docker
https://blog.csdn.net/vkingnew/article/details/85241600
## 查看docker版本
```
# docker -v
```
## 改名稱
```
# hostnamectl set-hostname centos7-1
//立即生效
# bash
```
## 改網卡
在右上角介面網路-設定-IPv4-Manual-Address[192.168.XXX.ooo(150)-255.255.255.0-192.168.XXX.2]-DNS(8.8.8.8)-apply-重開網路
## 連線可以用主機的名字
```
//第一台
# vim /etc/hosts
//輸入
192.168.xxx.150 centos7-1 centos7-1.test.com
~第二台
~第三台
:wq
```
## 關閉SELinux及防火牆
```
SELinux

//查看是否開啟
# getenforce
//if Enforcing(強制)
# vim /etc/selinux/config
//改
disabled
# reboot

防火牆
# systemctl status firewalld
# systemctl stop firewalld
# systemctl disable firewalld

# systemctl status sshd
要開
```
## ssh無密碼登入
```
//第一台
# ssh-keygen
瘋狂Enter
//回家目錄(~)
# cd
# ls
//隱藏節點
# cd .ssh
# ls
產生公鑰跟私鑰
//copy
# ssh-copy-id root@centos7-2
yes
第二台超級使用者密碼
~第三台
//測試
# ssh root@centos7-2
//可以copy /etc/hosts
# scp /etc/hosts root@centos7-2:/etc/hosts
~第三台
```
## 開啟docker service
```
# service docker start
# service docker status
```
# 小心得
## IPv4
網際協定版本4（英語：Internet Protocol version 4，又稱網際網路通訊協定第四版）

模式
* Automatic (DHCP) — 如果您連接的網絡使用DHCP服務器分配IP地址，請選擇此選項。您無需填寫DHCP 客戶端 ID字段。
* Manual(手動) — 如果您想IP手動分配地址，請選擇此選項。
* Link-Local Only — 如果您要連接的網絡沒有DHCP服務器並且您不想IP手動分配地址，請選擇此選項。隨機地址將根據RFC 3927分配前綴169.254/16。
* Disabled — IPv4對此連接禁用。
## Netmask
## Gateway
## DNS
## 什麼是SELinux?
SELinux 的全名為 Security Enhanced Linux，顧名思義就是為 Linux 提供了更進一步的安全防護。

三種狀態
* `enforcing`：強制使用，一般正常的啟動都是這個狀態，SELinux 會如實的運作。
* `permissive`：寬容模式，在這個模式下，SELinux 會如 enforcing 模式一樣將各種訊息寫入 log 以供查詢， 但不會真的限制程式的存取，因此在安全防護上等於沒有，可以把它當成 debug 模式。
* `disabled`：停用。