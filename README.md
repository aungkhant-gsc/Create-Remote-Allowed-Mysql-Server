# Create-Remote-Allowed-Mysql-Server

## 1. update dependencies
````
sudo apt update
````

## 2. install Mysql-Server
````
sudo apt install mysql-server
````
when promts ask to confirmation to install
Do you want to continue? 
type `yes`

## 3. Start Mysql service
````
sudo systemctl start mysql.service
````
## 4. Enter to mysql CLI
````
sudo mysql
````
## 5. Change authentication parameter using ALTER query
````
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by 'userPasswordHere';
````
## 6. Exit from mysql CLI
````
exit
````
## 7. Run Mysql Secure Installation
````
mysql_secure_installation
````
> After that type password that you set in step 5

> Promts will ask if you want to configure the validate password plugin

> if you answer `yes`, it will ask again to select a level of password validation

> Keep in min, that if you enter `2` for strongest level you will receive when attempting to set any password which does not contain 
`number`, `upper and lower case letters` and `special characters`

> As a next question, it will ask you to change root password

> If you type yes, you can set new password and comfirm by retyping.

> After you setting new password, promt will confirm you, Do you wish to use this new password? You can type `yes` or `no` what ever you want.

> Next, It will ask to remove anonymous users. For development you can type `no`. For production I recommend to remove anonymous users.

> Next promt in important. `Disallow root login remotely?`, Normally root login should allowed to connect only form localhost.So I recommend to type `yes`

> And then It will ask to remove test database, just type `yes`

> Also `yes` for reload privilege table.

finally All done.

## 8. Enter again to mysql CLI
````
mysql -u root -p
````

then type your `root password`

## 9. Now let Create new User
````
CREATE USER 'usernamehere'@'localhost' IDENTIFIED BY 'yourPassword'
````
## 10. Alternatively you can also create user for remote
````
CREATE USER 'usernamehere'@'remote_server_ip' IDENTIFIED BY 'yourPassword'
````
## 11. Grant privileges to a user account for both root user and remote user
###### `Mean, allowing the user to perform certain actions on specified databases or tables`
````
GRANT CREATE, ALTER, DROP, INSERT, UPDATE, SELECT, REFERENCES, RELOAD on *.* TO 'rootusername'@'localhost' WITH GRANT OPTION;
````
````
GRANT CREATE, ALTER, DROP, INSERT, UPDATE, SELECT, REFERENCES, RELOAD on *.* TO 'remote_username'@'remote_server_ip' WITH GRANT OPTION;
````
``` NOTE!, you should only grant users the permissions thay need.```
## 12. Give all permission to root user
````
GRANT ALL PRIVILEGES ON *.* TO 'rootusername'@'localhost' WITH GRANT OPTION;
````
## 13. Reload the grant tables and apply any changes made to the user privileges
````
FLUSH PRIVILEGES;
````
## 14. Exit from mysql CLI;
```` exit ````
## 15. Change mysel server config file to allow connect from all IP address
### edit mysqld.cnf
   ```` 
    sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf 
   ````
### Find bind-address line and change to 0.0.0.0
### and save and exit
## 16. Restart Mysql Service
````
sudo systemctl restart mysql
````
## 17. Enable Firewall
````
sudo ufw enable
````
## 18. Allow ssh from firewall
````
sudo ufw allow ssh
````
## 19. Allow remote IP for mysql port 3306
````
sudo ufw allow from 'remote_server_ip' to any port 3306
````
## 20. Finally we can connect to our mysql server from remote
````
mysql -u remoteusername -h mysql_server_ip -p
````
