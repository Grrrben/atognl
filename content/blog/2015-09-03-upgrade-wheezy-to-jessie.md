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

`</p>
<pre>sudo mv /etc/apache2/apache2.conf /etc/apache2/apache2.backup</pre>
<p>`

`</p>
<pre>sudo cp /etc/apache2/apache2.conf.dpkg-dist /etc/apache2/apache2.conf</pre>
<p>`

`</p>
<pre>sudo service apache2 start</pre>
<p>`

Apache starte nu weer, ik had alleen nog geen localhost websites werkend. Dit komt door de manier waarop de nieuwe Apache conf file met de sites-available omgaat:

Oude situatie

`# Include the virtual host configurations:<br />
Include sites-enabled/`

Nieuwe situatie

`# Include the virtual host configurations:<br />
IncludeOptional sites-enabled/*.conf`

De oude versie gebruikte alle bestanden uit de sites-enabled map, de nieuwe vereist een .conf file.
  
Dus moet je de naam van de vhost bestandjes iets aanpassen, een .conf extensie erachter zetten, dus voor ieder bestandje in de map:

`</p>
<pre>sudo cp websiteconfig websiteconfig.conf</pre>
<p>`

Gevolgd door:

`</p>
<pre>sudo a2ensite websiteconfig.conf</pre>
<p>`

Na een reload werkten alle websites weer

`</p>
<pre>sudo service apache2 reload</pre>
<p>`

Het laatste probleem was de phpmyadmin, deze kon nog niet benaderd worden door apache. 

In de oude apache2.conf stond de volgende zin:

`</p>
<pre>Include /etc/phpmyadmin/apache.conf</pre>
<p>`

Dit is bij de nieuwe versie gewijzigd in:

`</p>
<pre># Include generic snippets of statements
IncludeOptional conf-enabled/*.conf</pre>
<p>`

Het betekend dat de config file van de `/etc/phpmyadmin/` locatie naar `/etc/apache2/conf-available/` moet worden gekopieerd om hem vanuit daar toe te voegen.

`</p>
<pre>sudo cp /etc/phpmyadmin/apache.conf /etc/apache2/conf-available/phpmyadmin.conf</pre>
<p>`

`</p>
<pre>sudo a2enconf phpmyadmin.conf</pre>
<p>`

`</p>
<pre>sudo service apache2 reload</pre>
<p>`

En klaar, alles werkt weer prima.