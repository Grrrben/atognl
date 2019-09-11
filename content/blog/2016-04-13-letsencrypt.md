---
title: Letsencrypt
author: grrrben
type: post
date: 2016-04-13T09:22:54+00:00
url: /blog/letsencrypt/
categories:
  - security
tags:
  - https
  - ssl

---
Letsencrypt, de Certificate Authority van EFF, is uit de beta. Letsencrypt laat je &#8211; gratis &#8211; op websites beveiligde verbindingen instellen.

Als je zelf websites hebt die je via https wilt laten lopen is het een aanrader. Ze hebben voor Debian/Apache een commmand line client die erg straight-forward is waardoor je via ssh de ssl snel kunt instellen.<!--more-->

Allereerst heb je het tooltje nodig.

> &#8220;If your operating system includes a packaged copy of the letsencrypt client, install it from there and use the letsencrypt command. Otherwise, you can use our letsencrypt-auto wrapper script to get a copy quickly&#8221;

Maar een snelle blik in de apt-get van Debian geeft aan dat Lets Encrypt daar nog niet bijstaat. Maar misschien is het gewijzigd wanneer je dit leest, dus kijk zelf even.

`apt-cache search letsencrypt`

Als er niks terug gegeven wordt kun je de tool via github clonen. De code staat op <https://github.com/letsencrypt/letsencrypt>. Er staat een handleiding op de [website][1] die je uitlegt hoe je het makkelijk installeert via de terminal.

Het enige was dat het commando `letsencrypt` niet werkte en ik `letsencrypt-auto` moest gebruiken. Dus wijzig de commando&#8217;s als volgt:

`letsencrypt --apache`

`letsencrypt-auto --apache`

Gebruik je Apache als je http-server op een Debian install, dan gaat de installatie van de certificaten bijna automatisch. Je kunt een paar opties aanklikken in de tool, je kan bijvoorbeeld kiezen om alleen https verbindingen toe te staan, of ook http verbindingen (optionele ssh dus). De tool kijkt zelf al even welke vhost bestanden (lees: domeinen) er op de server draaien en laat je kiezen voor welke je certificaten wilt installeren. Voor de gekozen domeinen worden nieuwe config files in /etc/apache2/sites-available aangemaakt welke direct de juiste `SSLCertificateFile`, `SSLCertificateKeyFile` en `SSLCertificateChainFile` bevatten.

Hier is een screenshot van het tooltje via shh:

<a href="https://atog.nl/wp-content/uploads/2016/04/eff-tool.png" rel="attachment wp-att-217"><img class="alignnone size-full wp-image-217" src="https://atog.nl/wp-content/uploads/2016/04/eff-tool.png" alt="eff-tool" width="695" height="405" srcset="https://atog.nl/wp-content/uploads/2016/04/eff-tool.png 695w, https://atog.nl/wp-content/uploads/2016/04/eff-tool-300x175.png 300w" sizes="(max-width: 695px) 100vw, 695px" /></a>

Heb je &#8211; net als deze site &#8211; een WordPress installatie draaien dan moet je nog een paar aanpassingen doen. Eerst de url&#8217;s van de site zelf, dat gaat via instellingen > algemeen

`WordPress-adres (URL) - wijzig in https://<br />
Siteadres (URL) - wijzig in https://`

Daarna moet je nog wat harde links in de database aanpassen. WordPress heeft de eigenschap om images in wp-content/uploads met volledige url&#8217;s in de tabellen te zetten. Je kan het in de database zelf veranderen, maar het makkelijkste is een complete dump, in de gedumpte SQL de resources met http verwijzing in https te veranderen en dan de dump weer importeren via phpmyadmin. Maak even een backup, voor je importeert, voor de zekerheid.

Je kan het zien aan het slotje in de adresbalk van je browser. Bij mixed content (http en https) is deze grijs of geel, als alles goed staat is deze groen. Als je op het slotje klikt krijg je wat meer informatie over de verbinding. In dit geval staat er bovenaan een waarschuwing. Via het &#8220;Details&#8221; linkje geeft Chrome je nog wat extra informatie, en met de Network tab in de Developer tools kun je makkelijk zien om welke content het gaat. Je kunt filteren op `mixed-content:`.

<a href="https://atog.nl/wp-content/uploads/2016/04/partial-ssl.png" rel="attachment wp-att-216"><img class="alignnone size-full wp-image-216" src="https://atog.nl/wp-content/uploads/2016/04/partial-ssl.png" alt="partial-ssl" width="326" height="674" srcset="https://atog.nl/wp-content/uploads/2016/04/partial-ssl.png 326w, https://atog.nl/wp-content/uploads/2016/04/partial-ssl-145x300.png 145w" sizes="(max-width: 326px) 100vw, 326px" /></a>

In de SQL dump wijzig je `http://www.example.com/wp-content` in `https://www.example.com/wp-content`.

Na de upload van de dump naar de gekoppelde database. En klaar, je hele website draait nu op ssl.

<a href="https://atog.nl/wp-content/uploads/2016/04/fully-ssl.png" rel="attachment wp-att-215"><img class="alignnone size-full wp-image-215" src="https://atog.nl/wp-content/uploads/2016/04/fully-ssl.png" alt="fully-ssl" width="330" height="561" srcset="https://atog.nl/wp-content/uploads/2016/04/fully-ssl.png 330w, https://atog.nl/wp-content/uploads/2016/04/fully-ssl-176x300.png 176w" sizes="(max-width: 330px) 100vw, 330px" /></a>

[Letsencrypt website][2].

 [1]: https://letsencrypt.org/getting-started/
 [2]: https://letsencrypt.org/