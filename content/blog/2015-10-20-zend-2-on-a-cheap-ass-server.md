---
title: Deploying Zend 2 on a cheap-ass-server
author: grrrben
type: post
date: 2015-10-20T13:28:26+00:00
url: /blog/zend-2-on-a-cheap-ass-server/
categories:
  - php
tags:
  - framework
  - zend
  - zf2

---
I am involved in a ZF2 project that has to be deployed on a server where the directory structure can&#8217;t be changed. Normally you would just SFTP all your files and directories to your server, edit your apache conf file and tweak some settings and off you go. Your project, live in 5 minutes.<!--more-->

Since the hoster does not allow us to change the base directories we&#8217;ve got work to do:

&#8211; Restructure local directories
  
&#8211; Update the settings
  
&#8211; Keep everything nice and tidy in GIT
  
&#8211; Set project live

**But first things first**

We are going to make major changes. Let&#8217;s start with a new branch:

`git checkout -b structure`

**Restructuring**

The website does not have the directory structure as implemented by the ZF2 Skeleton application, which is something like:

`/config<br />
    ...<br />
/data<br />
    ...<br />
/module<br />
    ...<br />
/public<br />
    .htaccess<br />
    index.php<br />
    ...`

Instead, we have a www directory and a private directory. So, in order to make things work in a we will use the www directory as a designation for the local public directory.

Rename the directory using git:

`git mv public www`

Add it to the repository with the -u update flag

`git add -u www`

Next up, we want the application source under the private directory, supplied bij the hosting provider.

`mkdir private<br />
git add private`

Now we move our data to this new private directory, remember to use git&#8230;

`git mv config private/config<br />
git mv data private/data<br />
git mv module private/module<br />
git mv init_autoloader.php private/init_autoloader.php`

&#8230; and tell git about the changes

`git add -u private/config<br />
git add -u private/data<br />
git add -u private/module<br />
git add -u private/init_autoloader.php`

We&#8217;re making progress here. Check your `git status` for all the changes.

**Update the settings**

We are working on a .localhost website. So let&#8217;s edit apache so it knows about our new structure, mimicking the server settings.

`sudo nano /etc/apache2/sites-available/WEBSITE.conf`

Change the public to www here&#8230; And restart the server

`sudo service apache2 restart`

Now the website is reachable again, under the new settings. Of course it fails big time:

`Failed opening required 'config/application.config.php'`

So let&#8217;s edit the settings in Zend framework.

First up; the index.php file in www.

Remove the following line:

`chdir(dirname(__DIR__));`

Alter the path to the autoloader by prepending ../private:

`require '../private/init_autoloader.php';`

Alter the last line to include the private directory:

`Zend\Mvc\Application::init(require '../private/config/application.config.php')->run();`

Next we will tell our config file about the changes in our directory structure

Alter the module\_paths and the config\_glob_paths in private/config/application.config.php

`'module_paths' => array(<br />
    '../private/module',<br />
    '../private/vendor',<br />
),`

`'config_glob_paths' => array(<br />
    '../private/config/autoload/{{,*.}global,{,*.}local}.php',<br />
),`

Depending on the placement of your framework files, you may have to alter the ZF2_PATH. This is also done in the application.config.php file.

**Security**

We have put the application files in in the private directory for security reasons. A nice finishing touch is a small .htaccess file telling the server to deny all (public) access to these files. Create the .htaccess with &#8220;Deny from all&#8221; as the only line or just:

`echo "Deny from all" >> /private/.htaccess`

And add it to the repository&#8230;

`git add private/.htaccess`

**Keep everything nice and tidy in GIT**

Throughout this tut we have told GIT about all out changes. It should be ok already. To make sure, just view the status in your favorite GIT client or just check it with the command

`git status`

We should have a new file

`new file: private/.htaccess`

Some modified files

`modified: private/config/application.config.php<br />
modified: www/index.php`

And a whole lot of renamed files.

Push the changes and pull them in your live branch if everything works out ok.

**Set project live**

And that&#8217;s about it. We can just add the content of the www and private directories in their online counterparts. Just be aware of the server specific settings as the database credentials and the ZF2_PATH.