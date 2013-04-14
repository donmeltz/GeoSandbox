# TODO #

Things to wrap up in the configuration, documentation, and ideas for additional software and functionality.

## Configuration ##

----------

### Configure the Ubuntu operating system ###

----------

Nothing yet

----------

### Install and configure the  software ###

----------

Nothing yet

----------

### Configure data directories, users, and access permissions ###

----------

#### Configure users that will access Postgresql/PostGIS databases ####

TODO

#### Configure users that will access MySQL databases ####

TODO

----------

### Test the installed software ####

----------

#### MySQL ####

TODO

#### Postgresql/PostGIS ####

TODO

----------

## Documentation ##

(Document user names, passwords, and data directories)

### GeoWebCache 1.3 ###

TODO

### PostGIS 2.0 ###

TODO

## Other software: ##

### PostgreSQL 9.1 ###

TODO

### MySQL 5.5.24 ###

TODO

### PHP 5 ###

TODO

### Python 2.7.3 ###

TODO


## Additional Software and Functionality ##

----------

### Possible additional software and configurations ###

----------

#### Install TileStream (Have not got this to work yet)

(https://github.com/mapbox/tilestream)

In a terminal window - Install build requirements:

	sudo apt-get install curl build-essential libssl-dev libsqlite3-0 libsqlite3-dev git-core 

Install node:

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

Install npm: (not sure which one of the following lines to use)

	curl http://npmjs.org/install.sh | sh

	sudo apt-get install npm

Install TileStream:

	npm install -g tilestream

_Note from @notifyshane: @DonMeltz run 'npm install zlib' in /usr/local/lib/node_modules to fix the error. just figured it out. painful install on my mac._

#### Set up FTP (SFTP): ####

[https://help.ubuntu.com/11.04/serverguide/C/ftp-server.html](https://help.ubuntu.com/11.04/serverguide/C/ftp-server.html)

	sudo apt-get install vsftpd

Edit /etc/vsftpd.conf with these options:

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
    
Restart vsFTPd:

	sudo restart vsftpd