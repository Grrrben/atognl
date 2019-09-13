---
title: Upgrade Wheezy to Jessie
author: grrrben
type: post
date: 2015-09-03T12:16:59+00:00
url: /blog/upgrade-wheezy-to-jessie/
categories:
  - linux
tags:
  - debian
  - jessie
  - upgrade
  - wheezy

---
Ik heb (eindelijk) mijn laptop van Debian Wheezy naar Jessie geupdate. Het verliep bijna probleemloos, een complimentje voor de mensen achter Debian, alleen Apache gaf problemen, waarschijnlijk omdat ik de oude apache2.conf file gehouden had.<!--more-->

Dit was op te lossen door:

De content van `/etc/apache2/apache2.conf.dpkg-dist` in `/etc/apache2/apache2.conf` te stoppen (kopieer de oude eerst naar een backup file)

`
sudo mv /etc/apache2/apache2.conf /etc/apache2/apache2.backup
`

`
sudo cp /etc/apache2/apache2.conf.dpkg-dist /etc/apache2/apache2.conf
`

`
sudo service apache2 start
`

Apache starte nu weer, ik had alleen nog geen localhost websites werkend. Dit komt door de manier waarop de nieuwe Apache conf file met de sites-available omgaat:

Oude situatie

`# Include the virtual host configurations:<br />
Include sites-enabled/`

Nieuwe situatie

`# Include the virtual host configurations:<br />
IncludeOptional sites-enabled/*.conf`

De oude versie gebruikte alle bestanden uit de sites-enabled map, de nieuwe vereist een .conf file.
  
Dus moet je de naam van de vhost bestandjes iets aanpassen, een .conf extensie erachter zetten, dus voor ieder bestandje in de map:

`
sudo cp websiteconfig websiteconfig.conf
`

Gevolgd door:

`
sudo a2ensite websiteconfig.conf
`

Na een reload werkten alle websites weer

`
sudo service apache2 reload
`

Het laatste probleem was de phpmyadmin, deze kon nog niet benaderd worden door apache. 

In de oude apache2.conf stond de volgende zin:

`
Include /etc/phpmyadmin/apache.conf
`

Dit is bij de nieuwe versie gewijzigd in:

`
# Include generic snippets of statements
IncludeOptional conf-enabled/*.conf
`

Het betekend dat de config file van de `/etc/phpmyadmin/` locatie naar `/etc/apache2/conf-available/` moet worden gekopieerd om hem vanuit daar toe te voegen.

`
sudo cp /etc/phpmyadmin/apache.conf /etc/apache2/conf-available/phpmyadmin.conf
`

`
sudo a2enconf phpmyadmin.conf
`

`
sudo service apache2 reload
`

En klaar, alles werkt weer prima.