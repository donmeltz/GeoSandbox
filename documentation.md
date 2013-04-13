Documentation
=============

This page is most definitely incomplete. I will be updating it as time allows.

Operating system
----------------

### Ubuntu 12.04 ###

Default user: ubuntu

Password: geosandbox

To change the default password, enter the command:

    passwd

and follow the prompts

To add additional user(s) that can login to the server, enter the command:

    sudo adduser <newuser>

where <newuser> is the name you want to use for the new user.

Enter a password when asked, and any of the other optional information

Web server 
-----------

### Apache 2.2 ###

The root directory for all website files is: /var/www/

Members of the "www-pub" group have access and editing permissions for all files in the /var/www/ directory

The ubuntu user is a member of the www-pub group

To add users to the www-pub group, use the following command:

	sudo usermod -a -G www-pub <newuser>

Geo services 
-------------

### Open GeoSuite 3.0 ###

The default data directory for both geoserver and geoexplorer is: /usr/share/opengeo-suite-data

Members of the "geo-pub" group have access to all files in this directory

The ubuntu user is a member of the geo-pub group

To add users to the geo-pub group, use the following command:

	sudo usermod -a -G geo-pub <newuser>

### GeoServer 2.2 ###

Default user: admin

Password: geoserver

### GeoExplorer ###

Default user: admin

Password: geoserver

### GeoWebCache 1.3 ###

TODO

### PostGIS 2.0 ###

TODO

Other software:
--------------- 

### PostgreSQL 9.1 ###

TODO

### MySQL 5.5.24 ###

TODO

### PHP 5 ###

TODO

### Python 2.7.3 ###

TODO