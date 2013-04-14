# Documentation #

This page is most definitely incomplete. I will be updating it as time allows.

The purpose of this file is to document the user names, passwords, and data directories used by the various software programs installed on the system.

## Operating system ##

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

## Web server ##

### Apache 2.2 ###

The root directory for all website files is: /var/www/

Members of the "www-pub" group have access and editing permissions for all files in the /var/www/ directory

The ubuntu user is a member of the www-pub group

To add users to the www-pub group, use the following command:

	sudo usermod -a -G www-pub <newuser>

## Geo services ##

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
