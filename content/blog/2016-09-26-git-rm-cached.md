---
title: git rm –cached
author: grrrben
type: post
date: 2016-09-26T08:30:42+00:00
url: /blog/git-rm-cached/
categories:
  - progress
tags:
  - git
  - ignore

---
Met `git rm --cached [filename]` kun je een bestand uit de repository halen zonder dat deze ook echt van je computer verwijderd wordt. Het is een handig commando voor als je bijvoorbeeld al enkele bestanden of mappen hebt gepushed die nog niet in een `.gitignore` file zaten, maar daar eigenlijk wel thuis horen.<!--more-->

Als je je wijzigingen al gepushed hebt, en daarna de `.gitignore` aanpast, blijven al die bestanden zichtbaar in de pull-request en dergelijk. Dan kun je `git rm --cached [filename]` uitvoeren en dat committen & pushen om je code weer netjes te maken. 

Een bestand verwijderen

`git rm --cached bestand.txt`

Meerdere bestanden

`git rm --cached bestand.txt bestand2.txt dir/bestand-in-dir.txt`

Hele directory ineens

`git rm -r --cached dirname`