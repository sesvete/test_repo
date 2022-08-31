# Documenting things, that were done to set up the workspace

## GitHub

- seting up a [GitHub](https://github.com/) account and downloading the desktop companion app
- downloading the [publishwall project](https://github.com/kr1stjans/publishwall) to the Hard drive

## Phpstorm
- downloading and instaling [phpstorm](https://www.jetbrains.com/phpstorm/)

## XAMMP
- downloading [XAMPP](https://www.apachefriends.org/) and instaliing
- running Apache and MySQL to check if it works

![xampp](/home/simon/Projekt PublishWall/xampp_setup.png "xampp")

- for ubuntu
```
sudo /opt/lampp/lampp start
``` 

- it runs!

```
Make sure to download XAMPP with php version 7.4. to avoid issues that will be discused later in the document 
```  

## Setting up the development base in phpstorm
- download the [pw_light sql file](https://drive.google.com/file/d/1UaWXyZwcsjMEmN9U83q_4UFqGE7M3ZAl/view)

### Connecting phpstrorm to mysql
- open database tab in php storm and create a new database

![new_database](/home/simon/Projekt PublishWall/new_database.png "new_database")

- go to datasource and select MariaDB

![mariaDB](/home/simon/Projekt PublishWall/Maria_db.png "mariaDB")

- name should be @localhost for now (we wil change that later), host is localhost, port is 3306, user and pasword are wahever you set up while installing XAMPP
```
by default the username is root and the pasword is empty
```
- press test connection to see if it works

![test_connection](/home/simon/Projekt PublishWall/test_connection.png "test_connection")

- now we must create a pw_light database
- right click on local host - select new - query console

![query_console](/home/simon/Projekt PublishWall/query_console.png "query_console")

- type in console:
```
CREATE DATABASE pw_light;
USE pw_light;
```
- press run (the green button)
- right click on pw_light - go to import/export - restore with mysql

![restore_mysql](/home/simon/Projekt PublishWall/restore_mysql.png "restore_mysql")

- path to mysql is the mysql executable, path to dump is the path of our sql file

![restore](/home/simon/Projekt PublishWall/restore.png "restore")

- lastly go to properties and set as follows
```
Name: pw_light@localhost
database: pw_light
```

- if everything is working you should be able to see tables on the right

![tables](/home/simon/Projekt PublishWall/tables.png "tables")

# Testing XAMPP
- create a sybolic link between the publishwall folder, that was dowloaded from github and /opt/lampp/htdocs/
```
in terminal write:

ln -s ~/publishwall /opt/lampp/htdocs/
``` 
- display the webpage by typing http://localhost/publishwall/ into the browser

![prvi error](/home/simon/Projekt PublishWall/slika1.png "prvi error")

- you should get something like this

- go to publishwall/applications/appliation/config/ find the config_template.php file
- make a new file called config.php and copy config_template into it
- make the following changes inside the file
```
$config['base_url']    = 'http://localhost/publishwall';
$config['pw2_url'] = 'http://localhost/publishwall/';
$config['db_hostname'] = 'localhost';
$config['db_username'] = 'root';
$config['db_password'] = 'root';
$config['db_database'] = 'pw_light'
```
- you should get the following error

![error2](/home/simon/Projekt PublishWall/Brez naslova2.png "error2")

## Installing composer
- download and install [composer](https://getcomposer.org/doc/00-intro.md)
- inside the terminal write:
```
composer
``` 
- now we know it works

![composer](/home/simon/Projekt PublishWall/composer.png "composer")

- inside terminal move to:
```
~/publishwall/applications/application/libraries 
``` 
run:
```
composer install
```
- refresh http://localhost/publishwall

![php error](/home/simon/Projekt PublishWall/Screenshot from 2022-08-31 10-10-50.png "php error")

```
if the following error is displayed, that means xampp isn't using the correct php version. We need php 7.4, let's check which version xampp is using
``` 
- inside the publishwall folder find index.php
- insert the following inside the file:
```
phpinfo();exit(1);
```
![phpinfo](/home/simon/Projekt PublishWall/phpinfo.png "phpinfo")

-reload http://localhost/publishwall

![php8](/home/simon/Projekt PublishWall/Screenshot from 2022-08-31 10-57-17.png "php8")

```
In our case XAMPP is using php 8.1, we need php 7.4
```
- download [XAMPP](https://www.apachefriends.org/download.html) with php version 7.4 for your os
- install XAMPP
- open /opt/lampp/etc/extra/httpd-xampp.conf
- check if the following line is present
```
LoadModule php7_module        modules/libphp7.so
``` 
- to run the desired version, simply comment(#) the undesired one
```
LoadModule php7_module        modules/libphp7.so
   #LoadModule php5_module        modules/libphp5.so
```
- run XAMPP and reload http://localhost/htdocs

![php7](/home/simon/Projekt PublishWall/php7.png "php7")

- from index.php remove
```
phpinfo();exit(1);
```
- reload http://localhost/publishwall

![databaseerr](/home/simon/Projekt PublishWall/database_error.png "databaseerr")

```
By reinstaling WAMPP we messed up with the MySQL and our previous databases
``` 
- check if the table in question is present with the following command inside phpstorm console
```
select count(*) from wallpost
```
- in our case it displayed the following
```
[2022-08-31 11:39:22] Connected
pw_light> use pw_light
[2022-08-31 11:39:22] completed in 3 ms
pw_light> select count(*) from wallpost
[2022-08-31 11:39:23] [42S02][1932] Table 'pw_light.wallpost' doesn't exist in engine
```
- we need to remove the current database and load it again
- rigth click on pw_light and select drop

![drop](/home/simon/Projekt PublishWall/drop.png "drop")

- if it doesn't work we need to update MySQL
- go to
```
/opt/lampp/bin/
```
- inside the terminal write and run
```
sudo ./mysql_upgrade -u root -p
``` 
- try to drop the database again, if it doesn't work, we will have to delete in manualy
- go to /opt/lampp/var/mysql/
- delete the pw_light database manually with:
```
sudo rm -r pw_light/
``` 
- after dropping the database get access to the database with the process described in the document above
- finaly reload http://localhost/publishwall

![konec](/home/simon/Projekt PublishWall/konec.png "konec")

- yep to ne dela