# Creating a new Omeka instance on omekax.grinnell.edu

The latest release of Omeka is available on the omekax server as a zipped archive. The archive is located alongside the site directories in /var/www/html/
Creating a new Omeka instance involves extracting the archive into a new site directory, creating a database and database user for the new site, and completing the Omeka install script. Instructions for each of these steps follow. To complete these steps the administrator will need to have an account created on omekax.grinnell.edu and that account will need to be added to the SSH, sudoers, and omekaadmins groups.



SSH to the omekax.grinnell.edu server and change to the /var/www/html directory.
```cd /var/www/html```

extract omeka archive to new folder
```sudo unzip omeka-2.4.1.zip ```

rename folder to new site slug
```sudo mv omeka-2.4.1 {NewSiteDir}```

run mysql as root (request password from Mike C.)
```mysql -u root -p```

create new mysql user to be associated with the new site	
```CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';```

create new mysql database for new site	
```CREATE DATABASE newdb;```

grant the new user all privileges on the new database 
```GRANT ALL PRIVILEGES ON `newdb` . * TO 'newuser'@'localhost';
exit;```

edit db.ini of new site folder
```cd /var/www/html/{NewSiteDir}```
```sudo vi db.ini```

db.ini should contain:
```
[database]
host     = "localhost"
username = "newuser"
password = "password"
dbname   = "newdb"
prefix   = ""
charset  = "utf8"
```


Change owner:group of new site directory and enclosed files/directories
```cd /var/www/html/{NewSiteDir}```
```sudo chown -R vagrant:www-data .```

Change permissions for files folder so that new files can be uploaded:
```sudo chmod -R 664 files```

Change permissions for plugins folder so that administrators can upload plugins via sftp:
```cd /var/www/html/{NewSiteDir}/plugins```
```sudo setfacl -m g:admins:rwx .```
