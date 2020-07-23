# MYSQL 


## How to install it on Centos 7



`# yum localinstall https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm `


If you already install mysql other version on your centos , You should remove it firstly 
`rpm -qa | grep -i mysql`
`yum remove mysql*`




## Configure it 

1. change root paswd 

    - open & edit `/etc/my.cnf` or `/etc/mysql/my.cnf`
    - add `skip-grant-tables` under [mysqld]
    - restart Mysql
    - login to mysql `mysql -u root -p `
    - Run `mysql> flush privileges;`
    - Set new password by `ALTER USER 'root'@'localhost' IDENTIFIED BY 'NewPassworkd';`
    - GO back to /etc/my.cnf and **remove/comment** skip-grant-tables
    - Restart Mysql 
    - Now You will be able to login with the new password `mysql -u root -p `      



2. create some user  





3. change configure file 