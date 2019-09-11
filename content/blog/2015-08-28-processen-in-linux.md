---
title: Processen in Linux
author: grrrben
type: post
date: 2015-08-28T07:11:30+00:00
url: /blog/processen-in-linux/
categories:
  - linux
tags:
  - cmd
  - linux
  - terminal

---
De huidige processen op een Linux OS krijg je overzichtelijk met het programma `top`. Volgens de manual:

`top - display Linux processes`

[<img class="alignnone size-full wp-image-18" src="https://www.atog.nl/wp-content/uploads/2015/08/Screenshot-from-2015-08-28-085839.png" alt="Screenshot from 2015-08-28 08:58:39" width="737" height="497" srcset="https://atog.nl/wp-content/uploads/2015/08/Screenshot-from-2015-08-28-085839.png 737w, https://atog.nl/wp-content/uploads/2015/08/Screenshot-from-2015-08-28-085839-300x202.png 300w" sizes="(max-width: 737px) 100vw, 737px" />][1]

Het is de terminal versie van de processen die je op windows via ctrl-alt-del te zien krijgt.<!--more-->

`top` geeft informatie over de processen, het RAM geheugen en het gebruik van de swap. Onder die informatie geeft het een lijst met de huidige processen, inclusief de gebruiker die het process runt en de systeembronnen die het process gebruikt. Alles in real-time, en dat in de terminal.

Een process beëindigen kan binnen top via het `k` commando. De k-toets dus, gevolgt door het ID van het process. Het ID staat in de meest linker kolom, PID (Process ID).

Wil je even een snelle blik op bepaalde processen werpen dan is `ps` beter geschikt. Het is minder &#8220;fancy&#8221; dan `top`, geen realtime update bijvoorbeeld, maar dat maakt het overzicht rustiger.

`ps - report a snapshot of the current processes.`

Het `ps` commando geeft standaard alleen een lijst met de processen van het huidige terminal venster. Dat is vrij mager, waarschijnlijk wil je meer zien dan. Een lijst met _alle_ processen bijvoorbeeld:

<pre>ps aux</pre>

Dit commando geeft een duidelijk overzicht van alle processen met informatie over de user en tijd erbij. Bij ieder process staat een ID (process id), dit is de unieke identifier waarmee je het process kunt aanspreken.

ps kan soms een lange lijst geven. Om het overzichtelijk te houden kun je het commando pipen (uitbreiden met een extra commando). Bijvoorbeeld `more` of `less`:

<pre>ps aux | more
ps aux | less</pre>

Ben je op zoek naar iets specifieks, bijvoorbeeld omdat Iceweasel vastloopt, gebruik dan een pipe met grep:

<pre>ps aux | grep icew</pre>

`grep` is een afkorting van &#8220;Global Regular Expression Parser&#8221;, het programma print lijnen die aan een opgegeven regex pattern voldoen
  
Of nog sneller: `pgrep`

<pre>pgrep icew</pre>

En dan nu, omdat je het commando meestal gebruikt om een problematisch process te beeindigen:

<pre>kill PID_van_het_process</pre>

Dus bijvoorbeeld: `kill 1234`

Is een process hardnekkig, en lukt het kill&#8217;en niet direct gebruik dan:

<pre>kill -KILL PID_van_het_process</pre>

 [1]: https://www.atog.nl/wp-content/uploads/2015/08/Screenshot-from-2015-08-28-085839.png