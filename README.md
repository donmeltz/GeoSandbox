GeoSandbox
==========

What is it?
-----------

GeoSandbox is a personal playground; an online environment where I can try out the latest server and geo-centric software, and experiment with various website configurations and datasets. It's a safe place where I don't have to worry about what happens if something goes awry and brings the whole system down.

GeoSandbox is a place to learn. There are so many things that either have to, or could go into putting a slippy-map online, it boggles my mind. Really, the only way to learn how these things work together is to just dive in and start flailing around, trying not to drown in the process. Here's a sampling of the technology I'm talking about:

- TCP/IP
- HTTP
- Domains, DNS, Ports, and Routers
- Server operating systems
- Web server software
- Spatial data server software
- Settings, permissions, and logs
- HTML and CSS
- JavaScript, APIs, and other libraries
- PHP, Python, (and lately, Node.js)
- Compile/Build/Install routines for cutting edge stuff
- Website design
- Map layout and usability
- etc...


The GeoSandbox started life back in January of 2011 with a simple personal goal. I wanted to post a map online, and I wanted to do it for as close to free as possible. I took an old laptop I had lying around that wasn't doing anything, hooked it up to the internet, and started hacking away.

### The original GeoSandbox server: ###

![GeoSandbox - the original prototype](http://www.geosandbox.com/img/GeoSandbox_Original.jpg)

I started out with and old Dell Inspiron 600m laptop. It housed an Intel Pentium M 1.6 GHz processor with 1 GB of Ram running 32 bit Windows 7 Home Premium. If you want to read up on the details, you can take a look at a few of my blog posts, specifically:


[Serving Maps for Free](http://www.donmeltz.com/serving-maps-for-free/)

[Setting up my GeoSandbox](http://www.donmeltz.com/setting-up-my-geosandbox/), and

[GeoSandbox grows up to be a “Real boy”](http://www.donmeltz.com/geosandbox-grows-up-to-be-a-real-boy/)

Where is it now?
----------------

GeoSandbox has gone through countless iterations, including moving from a Windows/IIS environment, to Ubuntu/Apache. The most recent change has been a move into the cloud, still Ubuntu/Apache, but using Amazon Web Services.

### The GeoSandbox server today: ###

![Amazon Racks](http://www.geosandbox.com/img/amazon-racks.jpg)

For a more thorough description of this transition, take a look at these three blog posts:


[Serving Maps – in the Cloud – for Free (part 1)](http://donmeltz.com/blog/serving-maps-in-the-cloud-for-free-part-1/)

[Serving Maps – in the Cloud – for Free (part 2)](http://donmeltz.com/blog/serving-maps-in-the-cloud-for-free-part-2/)

[Serving Maps – in the Cloud – for Free (part 3)](http://donmeltz.com/blog/serving-maps-in-the-cloud-for-free-part-3/)

In a sentence, I describe it as:

> A geo-centric cloud-based web-server using Amazon Web Services (AWS) 

Where will it go? 
------------------

Who knows? My intentions are to continue adding other geospatial functionality. I would like to get TileStream (or some other mbtiles server) set up so I can test how that works with TileMill. I'd also like to add some javascript libraries like leaflet, so I can experiment with them in setting up a custom web-map client.

### How can you use it? ###

I've outlined the specific steps needed to set up a GeoSandbox instance using an AWS server in the [geosandbox_configuration](https://github.com/donmeltz/GeoSandbox/blob/master/geosandbox_configuration.md) file. If you want to set one up for your own personal use, simply follow that procedure.

I've also begun investigating how to share AMIs of AWS instances. As it is now, I'm not sure an AMI is all that useful as the set up and configuration is fairly straight forward. However, if you want to use that as an option you can take a look at what I've got so far in the [AMI file](https://github.com/donmeltz/GeoSandbox/blob/master/ami.md).

Once you've got a GeoSandbox set up, take a look at the [Documentation file](https://github.com/donmeltz/GeoSandbox/blob/master/documentation.md) to see brief descriptions of the installed software, a list of pertinent default users, passwords, and other settings. 