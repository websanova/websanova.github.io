---
layout: master
title: Homestead with sFTP and Sublime Text
keywords: websanova, laravel, homestead, vagrant, nfs, speed, ftp, sftp, sublime, text
description: Using Homestead for Laravel development is great however there can be some issues with the NFS. A good option is to use sftp to avoid these issues.
date: Sep 10 2015
img: /wp-content/uploads/2015/laravel-homestead-with-sftp-and-sublime-text.png
permalink: /blog/laravel/laravel-homestead-with-sftp-and-sublime-text.html
---

I recently wrote about [speeding up Homestead on Windows using NFS](/blog/laravel/speeding-up-homestead-on-windows-using-nfs). However after a few days usage I started seeing some occasional lag when saving files. Perhaps I have too many folders in my projects directory. But also I realized there are things that don't need syncing such as the `.git` folder.

It's much better to just have a folder to folder sync for the files we want to run from inside of Homestead. After a bit of reading I realized a good simple approach is to just use a handy sftp plugin that comes with Sublime Text.

Just follow the steps below to get setup:

## Remove Shared Folders

Go to `scripts/homestead.rb` and remove or comment out the shared folder section.

~~~
#Register All of the Configured Shared Folders section.
#if settings.include? 'folders'
#  settings["folders"].each do |folder|
#    mount_opts = []
#
#    if (folder["type"] == "nfs")
#        mount_opts = folder["mount_opts"] ? folder["mount_opts"] : ['actimeo=1']
#    end
#
#    config.vm.synced_folder folder["map"], folder["to"], type: folder["type"] ||= nil, mount_options: mount_opts
#  end
#end
~~~

## Permissions

After the changes are made you will need edit the permissions on the directory you will keep your projects in inside of Homestead.

~~~
> vagrant up
> vagrant ssh

> sudo chown $user ~/Code
~~~

## Generate .ppk Public Key

If you setup Homestead already then you will already have a key. We will just need to convert it to the Putty `.ppk` format.

First we will locate our existing key.

~~~
> vagrant ssh-config

Host default
  HostName 127.0.0.1
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile C:/Users/rob/Homestead/.vagrant/machines/default/virtualbox/private_key
  IdentitiesOnly yes
  LogLevel FATAL
~~~

Fire up the Putty Key Generator tool and load the key from the location of the `IdentityFile`.

You can leave the password fields blanks since this is all just local. Then hit `Save Private Key` and you're done.

For reference the name doesn't matter, but you can just call it `homestead.ppk`.

## Install sFTP Sublime Plugin

Next we will install the [sFTP plugin](http://wbond.net/sublime_packages/sftp) for Sublime Text.

You can navigate to `Preferences`, `Package Control`, `Install Package`. Then type in `sftp`. Select the sftp package and it will automatically install.

## Configure sFTP Plugin

First we will need to generate the `sftp-config.json` for the folder we want to map. From the Sublime Text sidebar, right click and press the `Map to Remote` option.

You can leave the options as are and just edit the following to match your own settings.

~~~
"upload_on_save": true,   
"host": "127.0.0.1",
"user": "vagrant",
"port": "2222",
"remote_path": "/home/vagrant/Code/project.com",
"ssh_key_file": "C:\\Users\\rob\\Homestead\\.vagrant\\machines\\default\\virtualbox\\homestead.ppk",
~~~

Just match the `remote_path` and `ssh_key_file` paths. The key file is the one we generated from above.

## Upload Files

Now that we have the connection setup we will do our initial upload. We can just right click on our root folder and select `Upload Folder`.

This will take a few minutes so it's a good time to take a break. After it's done any file you save will automatically transfer over instantly. 

**NOTE:** If you perform any file operations that are outside of Sublime you will need to run the sync. You can just click on a parent folder and select the `Sync Local -> Remote` option. Fortunately this doesn't occur often enough to be annoying.

## Ignoring Files

There is already a nice list of files included so you probably don't need to make any changes. However if there are any other unnecessary files you can include them there.

## Conclusion

Since I've been using this approach I haven't had any lag issues. I also like that I only sync the files I need and it doesn't mess around with `.git` folders and such.

We can also host as many projects as we like without worrying about creating lag since the sync only occurs when and where we want it to occur.

Definitely a better approach then a shared folder.


