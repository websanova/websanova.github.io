---
layout: master
title: Speeding up Homestead on Windows Using NFS
keywords: websanova, laravel, homestead, nfs, speed
description: Homestead is a great way to get a packaged environment for your Laravel packages. However for Windows we will need to setup NFS to get it our page load times to an appropriate level.
date: Sep 07 2015
img: /img/logo-200x200.png
permalink: /blog/laravel/speeding-up-homestead-on-windows-using-nfs.html
---

I recently took the plunge into getting a Homestead vagrant box setup locally for development. I followed the instructions on the Laravel docs page and all went well. It took me only about ten minutes to get everything working within Homestead, awesome!

But wait, why is everything so slow? I started searching Google and it seems it's a common problem for Windows. It seems the trick is to enable `nfs` support for windows and boot your folders using the `nfs` file system.

It took a bit of searching and tinkering so thought I would share is it only takes a few simple steps

## Install NFS Plugin

Just run this install for the plugin from anywhere.

~~~
vagrant plugin install vagrant-winnfsd
~~~

## Update homestead.rb

There is some weird bug here, not sure why it hasn't been pushed into the main repo yet, but you will need to replace some code here (if not already live).

**homestead/scripts/homestead.rb**

Replace the following:

~~~
if settings.include? 'folders'
  settings["folders"].each do |folder|
    mount_opts = []

    if (folder["type"] == "nfs")
      mount_opts = folder["mount_opts"] ? folder["mount_opts"] : ['actimeo=1']
    end

    config.vm.synced_folder folder["map"], folder["to"], type: folder["type"] ||= nil, mount_options: mount_opts
  end
end
~~~

With this:

~~~
if settings.include? 'folders'
  settings["folders"].sort! { |a,b| a["map"].length <=> b["map"].length }

  settings["folders"].each do |folder|
    config.vm.synced_folder folder["map"], folder["to"], 
    id: folder["map"],
    :nfs => true,
    :mount_options => ['nolock,vers=3,udp,noatime']
  end
end
~~~

## Update Homestead.yaml

We can now use `nfs` in the folders section of the homestead config file.

~~~
folders:
    - map: ~\projects
      to: /home/vagrant/Code
      type: nfs
~~~

## Rebooting

You may also need a reboot at this point especially if you've been playing around to try and get this working.

First make sure you remove any vm in your `VirtualBox` and also check the `~\VritualBox VMs` folder to make sure everything is deleted.

After a reboot try `vagrant up` again and it should spin up a new box. I found when I added folders I had to go through this each time. Fortunately that doesn't happen often at all.

## Disable Sendfile

This is on a bit of a side note, but it's also recommended that `sendfile` for Nginx be disabled.

~~~
> sudo vi /etc/nginx/nginx.conf
~~~

And change `sendfile` from `on` to `off`.
