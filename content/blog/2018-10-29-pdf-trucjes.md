---
title: PDF trucjes
author: grrrben
type: post
date: 2018-10-29T18:37:18+00:00
url: /blog/pdf-trucjes/
categories:
  - linux
tags:
  - pdf

---
PDF&#8217;s mergen kan met \`pdfunite\` welke in de poppler-utils package zit (debian).

`pdfunite <input_1>.pdf <input_2>.pdf <output>.pdf`

Om van meerdere JPG&#8217;s een pdf te maken kun je \`convert\` gebruiken. 

`convert <in_1.jpg> <in_2.jpg> <output>.pdf`

Maar je kan ook een wildcard in de input parameter ingeven:

`convert <SCAN*.JPG> <output>.pdf`

Van pagina&#8217;s uit een PDF een nieuw document maken:

`gs -sDEVICE=pdfwrite -dNOPAUSE -dBATCH -dSAFER -dFirstPage=1 -dLastPage=4 -sOutputFile=<output>.pdf <input>.pdf`