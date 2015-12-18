---
layout: master
title: Setting Up Let's Encrypt SSL Certificates
keywords: ssl, certificate, http, https, encrypt, letsencrypt, letsencrypt-auto, 80, 443
description: A short guide on how to generate and install SSL Certificates using let's encrypt using letsencrypt command for both the GUI and the command line.
date: Dec 18 2015
img: /wp-content/uploads/2015/setting-up-lets-encrypt-ssl-certificates.png
permalink: /blog/security/setting-up-lets-encrypt-ssl-certificates.html
---

I decided to try the [Lets Encrypt](https://letsencrypt.org/howitworks/technology) today.  I always hated setting up SSL certificates so I was hoping this would finally provide an easy consistent solution. It didn't disappoint.

## Install

You will just need to pull in the client to run the `letsencrypt-auto` executable.

~~~
> apt-get install git
~~~

~~~
> git clone https://github.com/letsencrypt/letsencrypt letsencrypt
~~~

## GUI Setup

If we run the `letsencrypt-auto` command without any options it will run a little wizard to guide you through setting up SSL certificates for your domains.

~~~
> cd letsencrypt
> ./letsencrypt-auto
~~~

The interface will ask you for an email and to select the list of domains you want SSL certificates for. I'm using Apache on Ubuntu and for me this process could not have been any easier. In literally about 30 seconds I had SSL support for my domain up and running live. Awesome.

## Command Line Setup

I did experience one issue with `http` not redirecting to `https` which I cover below. In the end you will probably want to create some kind of script to automate this whole process. So it's good to know how to run this single command line.

~~~
./letsencrypt-auto certonly --apache --agree-tos --email=rduchnik@gmail.com -d doitwithlaravel.com -d www.doitwithlaravel.com
~~~

The command creates the certificates only for use with Apache web service.

If you need specific instructions for your setup just follow the [letsencrypt quick start guide](https://community.letsencrypt.org/t/quick-start-guide/1631) which is very useful and easy to read.

## Debugging

From what I can gather `letsencrypt` sniffs out your domains config. In my case I'm using Apache on Ubuntu, so it just copied my config from the `sites-available` directory and added some paths for the SSL certificates.

Before:

~~~
<VirtualHost *:80>

    # Port 80 config.

</VirtualHost>
~~~

After:

~~~
<IfModule mod_ssl.c>
<VirtualHost *:443>

    # Port 80 config.

    SSLCertificateFile /etc/letsencrypt/live/websanova.com/cert.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/websanova.com/privkey.pem
    Include /etc/letsencrypt/options-ssl-apache.conf
    SSLCertificateChainFile /etc/letsencrypt/live/websanova.com/chain.pem
</VirtualHost>
</IfModule>
~~~

Nothing too crazy. If you get some errors it's a good chance the `letsencrypt` client could not figure out something with your configuration. You can just add those four lines manually your self. Of course just update the domain name.

For instance when I ran mine, it didn't setup the redirect from `http` to `https` properly. Since we don't really need the `VirtualHost *:80` container anymore we can pretty much replace it with just the redirect.

~~~
<VirtualHost *:80>
	ServerName websanova.com
    ServerAlias www.websanova.com

    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>
~~~

## Sites Enabled

It will really depend on your setup and what system you're using with Apache. In my case on Ubuntu it has the `sites-enabled` and `sites-available` folders. It created a separate file called `websanova.com-....conf` for the SSL configuration. I ended up deleting this and moving the entire config for both `http` and `https` into one config file under `websanova.com.conf`. 

~~~
<VirtualHost *:80>
	ServerName websanova.com
    ServerAlias www.websanova.com

    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<IfModule mod_ssl.c>
<VirtualHost *:443>

    # Old port 80 config.

    SSLCertificateFile /etc/letsencrypt/live/websanova.com/cert.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/websanova.com/privkey.pem
    Include /etc/letsencrypt/options-ssl-apache.conf
    SSLCertificateChainFile /etc/letsencrypt/live/websanova.com/chain.pem
</VirtualHost>
</IfModule>
~~~

## Notes

* The certificates only last for three months so you will have to repeat these steps every three months. There is work to have this fully automated so for the time being you will need to make sure you keep an eye on things so that your sites don't go down when the certificate expires. However with a command line setup this can be easily done through a simple shell script.

* One thing to be weary of if you're using Apache. If you set it up for only one domain, you will have one `VirtualHost` container setup in the config. Because of the way Apache works, if not match is found it will use the first one loaded. So this basically means you will need a `VirtualHost` directive for all domains if you don't want them defaulting to some other website.