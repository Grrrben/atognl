---
title: VPS installeren
author: grrrben
type: post
date: 2017-02-28T17:50:02+00:00
url: /blog/vps-installeren/
categories:
  - linux
tags:
  - deploy
  - vps

---
> NOTE TO SELF

Een korte samenvatting voor het installeren van een schone VPS image.<!--more-->

**SSH**

SSH is een stuk veiliger als je geen root toelaat. Daarvoor wijzig je bestand \`/etc/ssh/sshd_config\`.

<pre>PermitRootLogin no</pre>

Daarna de service herstarten.

<pre>/etc/init.d/ssh restart</pre>

Er zijn hele betogen geschreven over het wel of niet wijzigen van de poort. Mocht het gewenst zijn dan is het in het zelfde bestandje aan te passen.

**Firewall**

Beetje leesvoer: <https://help.ubuntu.com/community/UFW>. Maar kort samengevat komt het er op dat je met de UFW alle inkomende verkeer blocked, tenzij je dat met regels aanpast.

Iedereen moet bijvoorbeeld bij je websites kunnen:

<pre>sudo ufw allow 80/tcp</pre>

Ook als het om SSL gaat:

<pre>sudo ufw allow 443/tcp</pre>

En jij alleen bij SSH:

<pre>sudo ufw allow from &lt;ip&gt; to any port 22</pre>

Checken of alles goed staat:

<pre>sudo ufw status</pre>

Herstarten gaat via aan/uit:

<pre>sudo ufw disable
sudo ufw enable</pre>

**FTP**

De group webmasters mag bij /var/www:

<pre>sudo chgrp -R webmasters /var/www
sudo find /var/www -type d -exec chmod g=rwxs "{}" \;
sudo find /var/www -type f -exec chmod g=rws "{}" \;</pre>

De FTP user behoort tot de webmasters.

<pre>sudo adduser &lt;ftpusername&gt;
sudo adduser &lt;ftpusername&gt; webmasters</pre>

Verder is het het makkelijkst om ook de FTP via SSH te doen.

**Sites**

Backups van de apache2 kun je via SCP overzetten naar /etc/apache2/sites-available.

<pre>scp &lt;local_file&gt; &lt;user&gt;@&lt;host-ip&gt;:&lt;/path/remote_file&gt;</pre>

Tenzij je een Ubuntu versie hebt, die heeft geen echte superuser, zucht. Je kan het dan verplaatsen via SFTP en dan met een sudo -i op de juiste plek zetten.

**SSL**

Letsencrypt heeft de [Certbot][1] voor gemakkelijk certificaatbeheer. Een goed geplaatste cronjob zorgt voor automatische vernieuwing.

**PhpMyAdmin**

De halve wereld wil graag in je database rondneuzen. Een goed wachtwoord is het belangrijkst,  maar je kan ook makkelijk de /alias aanpassen.

Bovenaan bestand /etc/apache2/conf-available/phpmyadmin.conf kun je de alias wijzigen:

<pre>Alias /phpmyadmin /usr/share/phpmyadmin</pre>

 [1]: https://certbot.eff.org/