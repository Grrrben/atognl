---
title: Migratie van een Postfix server
author: grrrben
type: post
date: 2016-03-18T10:21:54+00:00
url: /blog/migratie-van-een-postfix-server/
categories:
  - linux
tags:
  - dovecut
  - e-mail
  - postfix

---
Ik heb een Postfix/Dovecot combinatie op een server draaien voor de mails. Vanwege een VPS switch moest dit naar de nieuwe server over gezet worden. En waar een migratie van een website altijd duidelijk is met vanzelfsprekende handelingen is een migratie van een e-mail server een wirwar van settings, lange droge documentatie steeds wisselende spam-richtlijnen.
  
<!--more-->

Het is zo&#8217;n klusje wat je maar gewoon moet doen. Dus met gezonde tegenzin:

Transip heeft een goede [tutorial][1] van de Postfix/Dovecot installatie staan. Deze is eigenlijk geschreven voor Ubuntu maar werkt ook op Debian. Het is een installatieinstructie die goed genoeg voor een private e-mailserver, voor een professionele emailserver heb je tegenwoordig zeker wat extra encryptie/beveiliging nodig.

De tutorial werkte niet (meer) out-of-the-box, er was een extra setting in de `/etc/postfix/main.cf` nodig; `smtpd_sasl_auth_enable = yes`

Het einde van de file ziet er dan zo uit:

    smtpd_sasl_auth_enable = yes
    smtpd_recipient_restrictions = permit_sasl_authenticated,permit_mynetworks,reject_unauth_destination
    smtpd_sasl_type = dovecot
    smtpd_sasl_path = private/auth-client

Dan de DNS settings. Als je op mail.domein.ext zit dan wordt het als volgt:

    Name TTL Type Value
    mail 1 day MX 10 @

Het duurt altijd even voordat de nameservers je DNS wijzigingen oppakken. Het kan helpen om de [TTL][2] van te voren even op een korte tijd te zetten (5 min) zodat de bovenstaande wijzigingen sneller opgepakt worden. 

Dan de bestaande mail overzetten. Postfix heeft een bepaalde directory structuur voor de mails, als je de mails van de bestaande server naar de nieuwe server kopieert dan staan daar je mails ook goed. 

Via scp kun je hele dir&#8217;s overzetten. Let op de -rp flags:

    $ scp -rp /home/MAILUSER/DOMAIN/HANDLE/ root@123.456.789.00:/home/MAILUSER/DOMAIN/ 

-r staat voor recursive, hele map met inhoud dus
  
-p behoud de tijden en rechten settings van de bronbestanden

MAILUSER is de user die je hebt aangemaakt om de mail te beheren, in het voorbeeld van de tutorial is het vmail.
  
DOMAIN bijvoorbeeld domein.ext
  
HANDLE gebruikersnaam, voor de @ in het e-mailadres
  
Let ook even op het ip adres achter root@.

Als je veel mailhandles (verschillende users) hebt kun je ook een stapje eerder beginnen, dus in plaats van de HANDLE kopieren, gewoon de hele DOMAIN dir met alle HANDLES.

`scp` is een beetje een [vreemde eend][3], het kopieert niet automatisch de verborgen bestanden mee. Daarvoor gebruik je:

    $ scp -rp /home/MAILUSER/DOMAIN/HANDLE/. root@123.456.789.00:/home/MAILUSER/DOMAIN/

De bestanden moeten allemaal van de juiste gebruiker zijn. Dus login op de nieuwe server en wijzig de owner met:

    $ sudo chown -R MAILUSER:MAILUSER /home/MAILUSER

De -R flag staat weer voor recursive.

Nu zou het moeten werken. Als je de gebruikersnamen en wachtwoorden identiek hebt hoef je de settings in een client (Icedove/Thunderbird) niet aan te passen. Je kan het nu eens testen, je mappen binnen de client horen alle mailtjes te bevatten en je zou mailtjes heen en weer moeten kunnen sturen. Om zeker te weten dat je al op de nieuwe server zit is het makkelijk om gewoon de oude server eens uit te zetten (stop). Postfix bestuur je met de volgende commando&#8217;s:

    $ sudo /etc/init.d/postfix start
    $ sudo /etc/init.d/postfix stop
    $ sudo /etc/init.d/postfix restart

Nog een SPAM dingetje. Als je geen reverse DNS ingesteld hebt kom je vaak niet door de spamfilters heen. Gmail geef dit terug als de reverse DNS niet goed staat:

`The sender does not meet basic ipv6 sending guidelines of authentication and rdns resolution of sending ip. Please review https://support.google.com/mail/answer/81126 for more information.`

Dus vergeet dat niet in te stellen. Instellen gaat waarschijnlijk via de settigns van je hostingprovider. ([Handleiding voor Transip][4])

 [1]: https://www.transip.nl/forum/post/prm/1500
 [2]: https://nl.wikipedia.org/wiki/Time_to_live#Time_to_live_in_het_DNS
 [3]: http://serverfault.com/a/21588
 [4]: https://www.transip.nl/vragen/46-hoe-mijn-reverse-dns-instellen/