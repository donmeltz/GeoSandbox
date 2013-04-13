GeoSandbox AMI 
===============

In order to save myself some time every time I want to start from scratch with a new, clean instance of the GeoSandbox, I've saved it as an AMI. This is a copy, or "image" of the GeoSandbox that I can just spin up, rather than going through all of the individual steps outlined on the Configuration page.

If you'd like to use this as an option, too, I've made it publicly available through this link:

[GeoSandbox AMI](https://console.aws.amazon.com/ec2/home?region=us-east-1#launchAmi=ami-ba8eead3)

If you use this AMI, and find any flaws in my procedure, or have some advice on how to make it more useful, please feel free to send me a note. I apreciate any feedback you can supply.

Some notes before you decide to go this route:
----------------------------------------------

- This is an exact copy of the GeoSandbox that I've set up using the steps on the Configuration page.
- This is NOT a secure server.
- I have removed the "authorized_keys" file from the /home/ubuntu/.ssh and /root/.ssh directories
- I have removed all key pairs from the /etc/ssh directory
- I have removed the bash history for both root and the ubuntu user
- However: user passwords are enabled, and...
- In most cases, the installed software uses default settings, including default passwords.
- For documentation about any deviation from the default settings, see the Configuration page and the Documentation page.
- The default password for the ubuntu user is "geosandbox".
- The security group used with this AMI should include the following ports opened: 22 (SSH to connect via SCP, or SFTP)
	- 80 (HTTP Apache)
	- 443 (HTTPS)
	- 8080 (HTTP OpenGeo Suite, tomcat)
	- 5432 (PostGIS)
	- 54321 (PostGIS, used by OpenGeoSuite)
	- 3306 (MySQL)