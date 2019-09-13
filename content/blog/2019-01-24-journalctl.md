---
title: journalctl
author: grrrben
type: post
date: 2019-01-24T10:56:18+00:00
url: /blog/journalctl/
categories:
  - linux
tags:
  - journalctl
  - log
  - systemd

---
Met `journalctl` kan je systemd journal/log bekijken en query’en.<!--more-->

`journalctl` geeft zonder parameters een overzicht van alle log messages, veel meer dan over het algemeen nodig is. Dus maak het wat specifieker door extra parameters aan `journalctl` mee te geven.

Met de `-u` flag kun je de naam van een service meegeven om alleen daarvan de logs te zien. Met de `-b` flag zie je alle logs _vanaf_ de laatste start van de computer.

journalctl -b -u &lt;ServiceName&gt;

Je kan de tijd nog iets strakker zetten, de logs sinds gisteren bijvoorbeeld:

journalctl -b -u &lt;ServiceName&gt; --since='yesterday'

Of het laatste uur:

journalctl -b -u &lt;ServiceName&gt; --since='1 hour ago'

Naast `--since` is er ook een `--until`. Beide werken, naast bovengenoemde voorbeelden, met `"yyyy-dd-mm hh:mm"` notatie.

Een van de meest gebruite parameters is de `-f` flag. Door deze mee te geven blijf je de output volgen, een beetje zoals `tail` werkt:

journalctl -f -u &lt;ServiceName&gt;

Om `journalctl` zonder `sudo` te gebruiken moet de user in groep `systemd-journal` zit.

`sudo adduser <user> systemd-journal`