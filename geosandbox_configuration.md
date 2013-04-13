Configuration
=================

Setting up the GeoSandbox
-------------------------

I've settled on the following process to set up a cloud server using [Amazon Web Services](https://aws.amazon.com/ "Amazon Web Services") (AWS). I use an [Ubuntu](http://www.ubuntu.com/ "Ubuntu") server operating system, with [Apache](http://httpd.apache.org/ "Apache Web Server") as the web server to host and deliver web pages and web applications. I use the [OpenGeo Suite](http://opengeo.org/technology/suite/download/ "OpenGeo Suite") as my main web mapping platform, which includes [GeoServer](http://geoserver.org/display/GEOS/Welcome "GeoServer") as the spatial data publisher.


### Set up an Ubuntu ami on AWS: ###

Sign into the Amazon Web Services account: [https://console.aws.amazon.com/console/home](http://aws.amazon.com/ "AWS Free Usage Tier")

Search for and install an appropriate ami from the Ubuntu Cloud Portal:[ http://cloud.ubuntu.com/ami/](http://cloud.ubuntu.com/ami/ "Ubuntu Cloud Portal") (in this case, us-east precise ebs) 

-   Click on the desired ami link (in this case, ami-a29943cb), which then opens the AWS Management Console Request Instances Wizard*   Make sure the instance type is what you want (I've found that micro works in most cases, but the OpenGeo Suite v3.0 requires adding a swap file)
-   Pick an availability zone (important if you want to easily move things around)
-   Enable termination protection
-   Add a Name tag for easy reference
-   Create a new key pair and save to appropriate directory (or use an existing key pair)
-   Set up a new security group (or use and existing group) with the following ports open
	-	22 (SSH - to connect via SCP, or SFTP)
	-   80 (HTTP - Apache)
	-   443 (HTTPS)
	-   8080 (HTTP - OpenGeo Suite, tomcat)
	-   5432 (PostGIS)
	-   54321 (PostGIS, used by OpenGeoSuite)
	-   3306 (MySQL)
-   Launch the new server
-   Add an Elastic IP to make it easier to connect to the server (just remember to reattach it every time the instance is stopped and restarted)

Connect to the new server using ssh, or an ssh enabled client (e. g. WinSCP, SECpanel).

The default user is &quot;ubuntu&quot;. Passwords are not enabled by default.


----------

### Configure the Ubuntu operating system ###

- Open a terminal window and enter:

    sudo apt-get update

    sudo apt-get upgrade

#### Give the default user (ubuntu) a password ####

    sudo passwd ubuntu

#### Enable Password Authorization ####

*   In a terminal window, open the sshd_config file in vi:

 sudo vi /etc/ssh/sshd_config

*   Enter INSERT mode by typing &ldquo;a&rdquo;
*   Using the arrow keys on the keyboard, scroll down to the  line that reads

PasswordAuthentication no

*   Right arrow over to the end of the line and backspace  twice to erase &ldquo;no&rdquo;
*   Type &ldquo;yes&rdquo;
*   Press the escape key on the keyboard to exit edit mode
*   Type &ldquo;:w&rdquo; and enter to save the file
*   Type &ldquo;:q&rdquo; and enter to exit VI
*   Apply the changes by restarting ssh

sudo /etc/init.d/ssh restart

#### Add a swap file to overcome the memory limitations of a micro instance ####

I've found that running the OpenGeo Suite on an AWS micro instance causes the tomcat server to quickly crash. Adding a swap file to the server appears to overcome this limitation.

*   Create a 1 GB storage file

    *   adjust the count= line to your liking

sudo dd if=/dev/zero of=/swapfile bs=1024 count=1048576

*   Turn this new file into a swap area

sudo mkswap /swapfile

*   Set the file permissions appropriately

sudo chown root:root /swapfile

sudo chmod 0600 /swapfile

*   Activate the swap area every time the system reboots by editing fstab (again, using vi)

sudo vi /etc/fstab

*   Enter INSERT mode by typing &ldquo;a&rdquo;
*   Move to the end of the file and then hit ENTER to make a new line
*   Add the following line:

/swapfile swap swap defaults 0 0

*   Press the escape key to stop editing
*   Type &quot;:wq&quot; then enter to save and exit vi
*   Reboot the server
*   Verify the swap file is activated

free -m

* * *

### Install and configure the  software ###

#### Install Apache server with MySQL and PHP ####

*   Open a terminal window and enter:

sudo apt-get install lamp-server^

*   or, you install each element one at a time:
            > #### Apache
> 
> sudo apt-get install apache2y
> 
> sudo /etc/init.d/apache2 restart
            > #### MySQL
> 
> sudo apt-get install mysql-server
            > #### PHP
> 
> sudo apt-get install libapache2-mod-php5
> 
> sudo a2enmod php5
> 
> sudo service apache2 restart

#### Install the OpenGeo Suite ####

_(Note: The OpenGeo Suite install includes Geoserver and Postgresql/PostGIS)_

*   In a terminal window, sudo to root:

sudo su

*   Import the OpenGeo GPG key:

wget -qO- http://apt.opengeo.org/gpg.key | apt-key add -

*   Add the OpenGeo APT repository:

echo &quot;deb http://apt.opengeo.org/suite/v3/ubuntu lucid main&quot; &gt;&gt; /etc/apt/sources.list

*   Update APT:

 apt-get update

*   Search for packages from OpenGeo:

 apt-cache search opengeo

*   If the search command does not return any results, the repository was not added properly. Examine the output of the apt commands for any errors or warnings.
*   Install the OpenGeo Suite package:

 apt-get install opengeo-suite

*   Reboot the server

#### Install  Webmin via APT:  [http://www.webmin.com/](http://www.webmin.com/) ####

*   Use vi to edit the /etc/apt/sources.list file:

sudo vi /etc/apt/sources.list                

*   Add these two lines:

deb http://download.webmin.com/download/repository sarge contrib

deb http://webmin.mirror.somersettechsolutions.co.uk/repository sarge contrib

*   Install the GPG key with which the repository is  signed, using command line:

sudo wget http://www.webmin.com/jcameron-key.asc

sudo apt-key add jcameron-key.asc

*   Install Webmin:

sudo apt-get update

sudo apt-get install webmin            

Access Webmin in a browser: http://&lt;your_server_ip&gt;:10000

*   Username: root (or) &lt;username&gt;
*   Password: &lt;usernamepassword&gt;

* * *

### Configure data directories, users, and access permissions ###

#### Website directories and users ####

*   Create a new group (www-pub) that will contain the user-editors of the website files

sudo groupadd www-pub

*   Add users that will edit website files to the new group

    *   use the &quot;-a&quot; switch to append to existing groups
*   change the &quot;ubuntu&quot; user to whatever other user you want to add

sudo usermod -a -G www-pub ubuntu

*   Check by displaying all groups for a particular user

groups ubuntu

*   Change the ownership of everything under /var/www to root:www-pub

sudo chown -R root:www-pub /var/www

*   Change the permissions of the folders to 2775

    *   Start with the /var/www folder:

sudo chmod 2775 /var/www

sudo find /var/www -type d -exec chmod 2775 {} \;

sudo find /var/www -type f -exec chmod 0664 {} \;

#### Geo-data directories and users ####

*   Create a new group (geo-pub) that will contain the user-editors of the geodata files

sudo groupadd geo-pub

*   Add users that will edit website files to the new group

    *   Use the &quot;-a&quot; switch to append to existing groups
*   Change the &quot;ubuntu&quot; user to whatever other user you want to add

sudo usermod -a -G geo-pub ubuntu

*   Check by displaying all groups for a particular user

groups ubuntu

*   Change the ownership of everything under /usr/share/opengeo-suite-data to tomcat6:geo-pub

sudo chown -R tomcat6:geo-pub /usr/share/opengeo-suite-data

*   Change the permissions of the folders to 2775

    *   Start with the <span class="code">/usr/share/opengeo-suite-data</span>:

sudo chmod 2775 /usr/share/opengeo-suite-data

sudo find /usr/share/opengeo-suite-data -type d -exec chmod 2775 {} \;

sudo find /usr/share/opengeo-suite-data -type f -exec chmod 0664 {} \;


#### Configure users that will access Postgresql/PostGIS databases ####

TODO

#### Configure users that will access MySQL databases ####

TODO

* * *

### Test the installed software ####

#### Apache ####

*   Test the Apache2 default website:  http://&lt;YourAWSPublicDNSorIP&gt;

#### OpenGeo Suite ####

*   Test the OpenGeo Suite: http://&lt;YourAWSPublicDNSorIP&gt;:8080/dashboard/
*   Launch GeoExplorer from the dashboard
*   Save the default map and exit Geoexplorer
*   View the map at: http://&lt;YourAWSPublicDNSorIP&gt;/geoexplorer/viewer#maps/1

#### MySQL ####

TODO

#### Postgresql/PostGIS ####

TODO

* * *

_Configuration last updated on 1/26/2013_

* * *

## Possible additional software and configurations ##

#### Install TileStream (Have not got this to work yet)

            > (https://github.com/mapbox/tilestream)

*   In a terminal window:*   Install build requirements:

sudo apt-get install curl build-essential libssl-dev libsqlite3-0 libsqlite3-dev git-core 

*   Install node:

git clone --depth 1   git://github.com/joyent/node.git

cd node

git checkout v0.4.9

export   JOBS=2 # optional, sets number of parallel commands.

mkdir ~/local

./configure --prefix=$HOME/local/node

make

make install

echo 'export   PATH=$HOME/local/node/bin:$PATH' &gt;&gt; ~/.profile

source ~/.profile

*   Install npm: (not sure which one of the following lines to use)

curl http://npmjs.org/install.sh | sh

sudo apt-get install npm

*   Install TileStream:

npm install -g tilestream

_Note from @notifyshane: @DonMeltz run 'npm install zlib' in /usr/local/lib/node_modules to fix the error. just figured it out. painful install on my mac._

#### Set up FTP (SFTP): ####

            > [https://help.ubuntu.com/11.04/serverguide/C/ftp-server.html](https://help.ubuntu.com/11.04/serverguide/C/ftp-server.html)

sudo apt-get install vsftpd

*   Edit /etc/vsftpd.conf with these options:

enable for standalone mode

                listen=YES
                tcp_wrappers=YES
                #
                # Access rights
                # Allow anonymous FTP?
                anonymous_enable=YES
                # Allow local users to log in
                local_enable=YES
                # Enable any form of FTP write command
                write_enable=YES
                # Default umask for local users is 077. Change this to 022
                local_umask=022
                #
                # Security
                force_dot_files=NO
                # Don't remap user name
                guest_enable=NO
                # Customize the login banner string
                ftpd_banner=Welcome to the UbuGeo FTP service
                # Limit user to browse their own directory only
                chroot_local_user=YES
                # Enable list of system / power users
                chroot_list_enable=YES
                # Actual list of system / power users
                chroot_list_file=/etc/vsftpd.chroot_list
                hide_ids=YES
                # pasv_min_port=50000
                # pasv_max_port=60000
                #
                # Features
                # Activate logging of uploads/downloads
                xferlog_enable=YES
                # Display directory listings with the time in&nbsp; your&nbsp;  local&nbsp; time&nbsp; zone
                use_localtime=YES
                ls_recurse_enable=NO
                ascii_download_enable=NO
                async_abor_enable=YES
                # Message greeting held in file .message or specify with  message_file=...
                dirmessage_enable=YES
                #
                # Performance
                one_process_model=NO
                idle_session_timeout=120
                data_connection_timeout=300
                accept_timeout=60
                connect_timeout=60
                max_per_ip=4
                #
                # nopriv_user=ftpsecure
                #
                # This option should be the name of a directory which is  empty
                # Also, the directory should not be writable by the ftp user
                # This directory is used as a secure chroot() jail at times  vsftpd does not require filesystem access
                secure_chroot_dir=/var/run/vsftpd/empty
                #
                # This string is the name of the PAM service vsftpd will  use.
                pam_service_name=vsftpd
                #userlist_enable=YES
                #
                # This option specifies the location of the RSA certificate  to use for SSL encrypted connections
                rsa_cert_file=/etc/ssl/private/vsftpd.pem

*   Restart vsFTPd:

sudo restart vsftpd