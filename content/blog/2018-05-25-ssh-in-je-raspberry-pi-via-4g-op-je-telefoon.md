---
title: SSH in je Raspberry Pi via 4g op je telefoon
author: grrrben
type: post
date: 2018-05-25T07:55:33+00:00
url: /blog/ssh-in-je-raspberry-pi-via-4g-op-je-telefoon/
categories:
  - linux
tags:
  - raspberry-pi

---
Ik heb zo&#8217;n Raspberry Pi waarbij ik SSH toegang nodig heb. Om een beetje te tinkeren. Vaak is VIM wel genoeg dus een GUI of beeldscherm op de Pi heb ik niet nodig.

Nu had ik mijn Pi en laptop via 4g aan het internet gezet en zocht ik de juiste IP voor de SSH toegang, maar hoe vind je die eigenlijk als je telefoon geen IP informatie geeft?<!--more-->

nmap laat je een netwerk scan doen, dus daar begin je mee:

`sudo apt install nmap`

`nmap -sP 192.168.1.*`

Maar dan moet je natuurlijk wel binnen de `192.168.1.*` range zitten, wat ik niet zat.

Dus eerst maar mijn eigen netwerk IP address vinden, van de laptop via welke ik toegang wil krijgen:

`sudo ip addr show`

Dat geeft je eigen IP terug in de wlan key:

`3: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000<br />
link/ether b8:27:eb:74:80:89 brd ff:ff:ff:ff:ff:ff<br />
inet 192.168.43.30/24 brd 192.168.43.255 scope global wlan0<br />
valid_lft forever preferred_lft forever<br />
inet6 fe80::11e8:36eb:c6e0:a1e0/64 scope link<br />
valid_lft forever preferred_lft forever<br />
` 

Deze regel, `inet 192.168.43.30/24 brd 192.168.43.255 scope global wlan0` geeft me genoeg informatie; de Pi zit vast ook in de `192.168.43.*` range waar ik zelf in zit. Dus nmap nog maar eens laten scannen:

`nmap -sP 192.168.43.*`

En ja, daar zijn we allebei:

`Nmap scan report for raspberrypi (192.168.43.30)<br />
Host is up (0.023s latency).<br />
Nmap scan report for debian (192.168.43.64)<br />
Host is up (0.0017s latency).<br />
`