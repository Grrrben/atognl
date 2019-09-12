---
title: Git, branchen en mergen
author: grrrben
type: post
date: 2015-09-15T13:30:19+00:00
url: /blog/git-branch-merge/
categories:
  - progress
tags:
  - git

---
[<img class="alignnone size-large wp-image-132" src="https://www.atog.nl/wp-content/uploads/2015/09/trains-1024x640.jpg" alt="trains" width="660" height="413" srcset="/images/wp-content/uploads/2015/09/trains-1024x640.jpg 1024w, /images/wp-content/uploads/2015/09/trains-300x188.jpg 300w" sizes="(max-width: 660px) 100vw, 660px" />][1]

Dit is een vervolg op de eerdere blog over de [GIT commando&#8217;s][2].

**Branch**

Het branch commando maakt nieuwe zijtakken (branches) binnen je git project.<!--more-->

<pre>git branch &lt;naam-nieuwe-vertakking&gt;</pre>

De branches werken &#8211; op de echte git manier &#8211; met pointers naar eerder commits. Het is dus niet zozeer een kopie van je repository, je moet het zien als een verwijzing naar een bepaald punt op de master branch (hoofdtak) waarnaar je verwijst.

Om een nieuwe branche aan te maken gebruik je het commando:

<pre>git branch new_feature</pre>

Let op; je blijft op de huidige branch zitten. new_feature wordt aangemaakt, maar je switcht niet direct.

Een branch aanmaken en switchen in één commando kan wel, hiervoor gebruik je de -b flag:

<pre>git checkout -b new_feature</pre>

Het is een shortcut voor:

<pre>git branch new_feature
git checkout new_feature</pre>

**Switchen van branch**

Switchen tussen de git branches doe je met het checkout commando:

<pre>git checkout new_feature</pre>

Met het branch commando &#8211; zonder argumenten &#8211; krijg je een lijst van de beschikbare branches.

<pre>git branch</pre>

Om te weten waar je bent, altijd de bekende status gebruiken.

<pre>git status</pre>

Feedback: On branch new_feature, met extra informatie over de status van de bestanden op je systeem vs de repository.

Alle add, commit en andere statements worden nu op de new_feature branch gedaan.

Goed aan de git werkwijze is dat ook je localhost en IDE van branch naar branch meeverhuizen. Je kan dus aan een feature werken en al je bestanden in je localhost website zien, ook je IDE laat al die bestanden zien. Switch je van branch, dan laten zowel localhost als de IDE direct de andere branch zien.

Werk je bijvoorbeeld aan de new\_feature branch en in een productie-omgeving is een bug die gefixed moet worden dan switch je (terug) naar de master branch, doet je wijzigingen terwijl de laatste ontwikkelingen uit new\_feature je niet tot last zijn, commit, test, publiceert en gaat vervolgens weer naar de new_feature branch om direct weer verder te gaan met de laatste ontwikkelingen.

<pre>git checkout master</pre>

<pre>git pull origin master</pre>

Doe hier al je wijzigingen en vervolgens iets als:

<pre>git add .</pre>

<pre>git commit -a</pre>

<pre>git push origin master</pre>

Na publicatie van de wijzigingen kun je weer verder op de new_feature branch, precies waar je eerder gebleven was.

<pre>git checkout new_feature</pre>

**Mergen van branches**

Ben je klaar met de development van de een nieuwe branch dan merge je deze in de master branch, de hoofdtak dus. Het is &#8211; zoals altijd &#8211; even belangrijk te weten waar je zit zodat je zeker van je commando&#8217;s bent. Dus begin met een git status om te kijken of je op de master bent. Zoniet dan kun je daar naartoe switchen en vanuit de master branch de merge doen.

<pre>git status</pre>

<pre>git checkout master</pre>

Omdat je van git status nooit genoeg kunt krijgen, bekijk ook hier even hoe het ervoor staat. Wil je misschien nog iets committen, pushen, etc.

<pre>git status</pre>

Om nu de aanpassingen van de new_feature branch in de master te stoppen ga je git merge gebruiken:

<pre>git merge new_feature</pre>

Dit moet natuurlijk nog wel gepushed worden naar de repository om alles up-to-date te houden. Git status zou op dit moment iets zeggen als:

On branch master
  
Your branch is ahead of &#8216;origin/master&#8217; by 5 commits.
  
(use &#8220;git push&#8221; to publish your local commits)
  
nothing to commit, working directory clean

Een push dus, richting de master branch:

<pre>git push origin master</pre>

Eventueel kun je na de merge een branch verwijderen met het branch -d commando.

<pre>git branch -d new_feature</pre>

**Samenwerken**

Branches die door anderen gecreert zijn kun je ophalen met de fetch functie.

<pre>git fetch</pre>

 [1]: https://www.atog.nl/wp-content/uploads/2015/09/trains.jpg
 [2]: http://www.atog.nl/blog/git/