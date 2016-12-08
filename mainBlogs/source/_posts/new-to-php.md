---
title: new to php
date: 2016-12-08 10:17:18
tags:
---
#Install PHP and etc using xampp on ubuntu
1. download and install xampp
```
sudo chmod +x xampp-linux-x64-*-installer.run
sudo ./xampp-linux-x64-*-installer.run
```
	XMAPP will be installed to opt/lampp
2. Helloworld of php
create index.php file and copy to apache httpd root, xampp sets default web root to /opt/lampp/htdocs.
you can change to any path if you want by edit /opt/lampp/etc/httpd.conf file.
Find DocumentRoot and the corresponding Directory
```
<?php
	echo '<p>Hello world</p>';
?>
```
	Then, restart apache, visit it with Chrome(localhost) to see the result

3. How to get variables of html input control
```
$tireqty					// short style
$_POST[‘tireqty’]			// medium style
$HTTP_POST_VARS[‘tireqty’]	// long style
``` 
