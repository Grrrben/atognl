---
title: "Van Wordpress naar Hugo"
date: 2019-09-10T11:47:18+02:00
url: /blog/from-wordpress-to-hugo/
type: post
---

Ik heb mijn blog naar Hugo gemigreerd!

Eigenlijk zijn de update frequenties van Wordpress 
en zijn dependencies gewoon een beetje te groot voor 
het blog. 

Atog is in de kern eigenlijk een soort kladblok voor me, 
met voornamelijk kleine post met een paar commando's voor
de linux CLI of programma's. 
Zo nu en dan een blogpost of een klein reisverslag maar dat is het ook.  

Wil je dat dan perse dynamisch hebben, of is statisch eigenlijk wel prima. 
Want reageren kan bij het laatste bijvoorbeeld niet. 
Maar goed, realistisch, hoeveel reacties kreeg ik. 
Dat zijn er maar weinig.
In elk geval te weinig om de verantwoordelijkheid van een dynamische website 
te willen dragen. 

Want je moet wel:  
- Reacties checken, wat voornamelijk het blokkeren van SPAM bots betekend.  
- Wordpress altijd updaten omdat de hele hacker (script kiddies) 
wereld altijd de Wordpress sites kraakt.  
- Je Wordpress plugins updaten  
- PHP draaien en updaten  
- Een database draaien en updaten

Dus ik ben naar [Hugo](https://gohugo.io) gegaan! 
En dat ging best simpel. 

Eerste heb ik in Wordpress een 
[exporteer plugin](https://github.com/SchumacherFM/wordpress-to-hugo-exporter) geinstalleerd 
die al mijn posts netjes als markdown in een zipje stopte. 
Er zijn een paar dingetjes waar je op moet letten; 
github gists werden niet ~~goed~~ naar wens overgenomen en 
je moet je images nog even goed zetten.

Maar daarna heb ik retesnel een Hugo site aangemaakt en een [thema](https://dashdashzako.github.io/hugo-journal-demo/) 
genomen dat zich makkelijk laat aanpassen. En dan ben je eigenlijk al klaar. 

Een middagje werk.



