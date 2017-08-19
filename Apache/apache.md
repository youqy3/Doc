## ubuntu下配置LAMP
- 安装Apache：sudo apt install apache2
- 安装MySQL：sudo apt install mysql-server mysql-client
- 安装phpmyadmin：sudo apt install phpmyadmin

## 配置Apache的DocumentRoot为自己的文件夹
-- 编辑/etc/apache2/apache2.conf文件，将documentroot修改为自己的文件目录
-- 编辑/etc/apache2/sites-available/000-default.conf文件，将documentroot修改为自己的文件目录
-- systemctl restart apache2.service重启apache2服务器
