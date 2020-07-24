# MYSQL 


## How to install it on Centos 7


1. **METHOD 1**

    `# yum localinstall https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm `


    If you already install mysql other version on your centos , You should remove it firstly.Run the fllowing command  

    ```bash 
    rpm -qa | grep -i mysql
    yum remove mysql*
    ```

2. **METHOD 2**

    - Download RPM package , From `mysql` Web site

        ```bash 
        wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-community-server-5.7.31-1.el7.x86_64.rpm  

        ``` 
    - Install it 

        ```bash
        sudo yum install <package_name>
        ```






## Configure it 

### 1.change root paswd 

    - open & edit `/etc/my.cnf` or `/etc/mysql/my.cnf`
    - add `skip-grant-tables` under [mysqld]
    - restart Mysql
    - login to mysql `mysql -u root -p `
    - Run `mysql> flush privileges;`
    - Set new password by `ALTER USER 'root'@'localhost' IDENTIFIED BY 'NewPassworkd';`
    - GO back to /etc/my.cnf and **remove/comment** skip-grant-tables
    - Restart Mysql 
    - Now You will be able to login with the new password `mysql -u root -p `      



### 2.create some user  

    ```bash 
    `mysql -u root -p -e "GRANT USAGE ON *.* 
    TO 'russell'@'localhost'
    IDENTIFIED BY 'your_paswd'";
    ```

### 3.change configure file 

- Set remote access 

    - open `/etc/my.cnf` file 
    - add some option, like the fllowing  

    ``` bash 
        [mysqld]
        user=[*your user*] 
        bind-address=192.168.***.***
        language = /usr/share/mysql/English 
    ```

    - create new databases and add new user can remote access this databases
        ```bash
        #create new DB
        >mysql CREATE DATABASE newdb;


        >mysql GRANT ALL ON newdb.* 
        >mysql TO 'username'@'%'
        >mysql IDENTIFIED BY 'newpassword'; 
        ```



## FAQ

1. Fail to start mysqld server 


    - **Problems:**
        ```log 
        log:
            MYSQL server start failed: InnoDB: Table flags are 0 in the data dicitionary but the flags in file ./ibdata1 are 0x4000! 
        ```
    - **The problem reason and How to solve it**

        Because of your database file . So you should delete them .
        ```bash
        cd /var/lib/mysql
        sudo rm -rf *
        sudo service mysqld restart 
        ```

2. Fail to Add user 
    - **Problems:** 
        ```log
        error:1819 
        ```
    - **Reasons and How to solve it**
        - Reason

        `Pasword not satisfy the current policy requirements`

        - Solve it 
        ```
        >mysql  SHOW VARIABLES LIKE 'validate_password [localhost or %]';
        >mysql  SET GLOBAL validate_password_length = 6;
        >mysql  SET GLOBAL validate_number_couent = 0;
        >mysql  SET GLOBAL validate_password.policy=LOW;
        >mysql  SET GLOBAL validate_password_special_count=0;
        ```
        - Refer 
        
            [REFER this](https://defragged.org/2020/02/29/mysql-error-1819-hy000-your-password-does-not-satisfy-the-current-policy-requirements/)

3. command command 

    - Browse all user name 

        `>mysql  SELECT User,Host, FROM mysql.user;`