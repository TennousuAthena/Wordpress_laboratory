# Wordpress_laboratory

# WordPress：从入门到精通

# 目录

1. 概述

---

## 安装
 - 示例系统： Ubuntu 18

### 0x01 准备工作：更换APT源

```bash
sudo mv /etc/apt/sources.list /etc/apt/sources.list.back #备份
sudo touch /etc/apt/sources.list #创建空白文件
#这样做不太稳，还是算了 echo -e "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse\ndeb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse\ndeb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse\ndeb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse\n" > /etc/apt/sources.list
sudo nano /etc/apt/sources.list
# 然后复制粘贴以下内容
```

*清华源Ubuntu镜像*

[https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)

```bash
# Nano Ctrl+O 保存 Ctrl+X 退出
sudo apt update && sudo apt -y upgrade #更新源与系统软件
sudo apt -y install software-properties-common #安装apt第三方库
sudo find /etc/apt/sources.list.d/ -type f -name "*.list" -exec  sed  -i.bak -r  's#deb(-src)?\s*http(s)?://ppa.launchpad.net#deb\1 https\2://launchpad.proxy.ustclug.org#ig' {} \; #替换为镜像
sudo add-apt-repository ppa:ondrej/php #添加php库
sudo apt update  #再更新下
```

### 0x02 安装Apache2

```bash
sudo apt -y install apache2
sudo systemctl start apache2 #自启
```

打开便可看到Apahce默认页

![WordPress%208ca7498473624fe09aebb5bc7d747de9/Untitled.png](https://i.loli.net/2020/06/14/Q9gTyw4ix7etfPK.png)

Apache2默认页

### 0x03 安装php7 & MYSQL

```bash
sudo apt -y install php7.4 php-mysql libapache2-mod-php mysql-server
sudo apt -y install php7.4-curl php7.4-dom php7.4-mbstring php7.4-imagick php7.4-zip php7.4-gd
php -v #查看php版本
sudo service mysql restart && sudo service apache2 restart

sudo mysql_secure_installation #初始化mysql
sudo mysql -u root -p #+刚才设置的密码 进入MYSQL
```

```sql
#SQL命令
CREATE DATABASE `wordpress`;
SHOW DATABASES;
CREATE USER `wp`;

SET PASSWORD FOR wp = PASSWORD("pswqwq");
GRANT ALL PRIVILEGES ON wordpress.* to wp IDENTIFIED BY "pswqwq";
FLUSH PRIVILEGES;
EXIT;
```

### 0x04 部署Wordpress

```bash
sudo chmod -c 777 /var/www/html && sudo chown -R www-data:www-data /var/www/html
cd /var/www/html
sudo wget https://cdn.jsdelivr.net/gh/qcminecraft/Wordpress_laboratory@master/release/wordpress-5.4.2-zh_CN.tar.gz
sudo tar -zxvf wordpress-5.4.2-zh_CN.tar.gz
sudo mv wordpress/* . && sudo rm -r wordpress/
sudo rm -f latest-zh_CN.zip
```
