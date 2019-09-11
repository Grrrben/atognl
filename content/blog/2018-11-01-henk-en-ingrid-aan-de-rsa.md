---
title: Henk en Ingrid aan de RSA
author: grrrben
type: post
date: 2018-11-01T18:49:36+00:00
url: /blog/henk-en-ingrid-aan-de-rsa/
categories:
  - golang
tags:
  - encryptie
  - golang
  - rsa

---
Henk en Ingrid hebben een mening. Een politiek geladen mening die niet door iedereen gewaardeerd wordt, daarom houden ze hem liever voor zichzelf. Om met elkaar te kunnen praten gebruiken ze encryptie, Henk moet zijn boodschap ongezien bij Ingrid kunnen krijgen, terwijl Ingrid wel in staat moet zijn Henk zijn boodschap te lezen. En vice versa natuurlijk, ook Ingrid heeft een foute mening.<!--more-->

Dit doe je met Public Key Cryptografie. Bij Public Key Cryptografie heb je een publieke sleutel die iedereen kan zien en een privé sleutel die alleen van jou is. De publieke sleutel geef je aan iedereen die jou een bericht wil sturen, je kan hem zelfs op het internet zetten. De privé sleutel deel je niet, deze gebruik je om een versleuteld bericht leesbaar te maken.

Gelukkig kan Henk een beetje programmeren! En een beetje is precies genoeg, want zoals Henk zegt:

> Wat weten ‘experts’ er nou van?

Dus daar gaan we. Met een kleine disclaimer trouwens: _Dit is een oefening in RSA encryptie in Golang. Het is niet gemaakt om in productie gebruikt te worden. En in het echt heet ik geen Henk._

Kijk mee in de code: Clone de repository met het [Golang RSA voorbeeld][1] met:
  
`git clone git@github.com:Grrrben/golang-rsa.git`

We beginnen met een RSA identiteit. Een simpele Go struct met  een publieke- en privé sleutel, die struct maakt duidelijk waar we over praten en we kunnen er verschillende methods aan hangen.:

{{< gist Grrrben 468ae117aead53847754fd2bf2261f2a "rsa_identity.go" >}}

Iedereen mag `Public` benaderen (let ook op de hoofdletter van Public, die de variabele in Go ook echt publiek maakt), `private` is alleen voor jou. Bij Public Key Cryptografie is het zo dat als Henk Ingrid een bericht wil sturen, hij de publieke sleutel van Ingrid gebruikt om het bericht te versleutelen:

{{< gist Grrrben 468ae117aead53847754fd2bf2261f2a "rsa_encrypt.go" >}}

Henk wil aan Ingrid een boodschap wil sturen:

> Negenennegentig procent van de problemen in de wereld worden veroorzaakt door buitenlanders.

Henk gebruikt daarvoor de publieke sleutel van Ingrid, met de bovenstaande Encrypt functie wordt het onleesbaar.

{{< gist Grrrben 5de0b636c6de138555209292407d5136 "rsa_encrypt_example.go" >}}

Onleesbaar dus, de zin wordt iets als:

_`zmNOOe@{��}9������#-��^� ����Jh��l0����c2S���a>�AA/�z�2��*c�@^w�P�Xu�HK���hu_u̴Jsi���7 [etc]`_

Sterker nog, iedere keer als je de zin versleuteld zal de uitkomst anders zijn. De test \`TestEncryptionNeverTheSame\` in de  testfile laat dit zien.

Niemand kan het lezen, behalve als je Ingrid bent, want dan ben je in het bezit van de privésleutel die deze tekst kan ontsleutelen.

{{< gist Grrrben 468ae117aead53847754fd2bf2261f2a "rsa_decrypt.go" >}}

In de repository van Github zitten een paar tests. Die draai je met `go test -v`. De method `TestEncryptDecrypt` geeft een voorbeeld van een ontsleuteling van de boodschap met `Decrypt`.

{{< gist Grrrben e74d0becb95aca59a1661a0745532106 "rsa_encrypt_decrypt.go" >}}

Wil je wat meer informatie zien dan kun je in de testfile altijd even wat variabelen dumpen met `fmt.Println(var)`. Om de verschillende Messages te dumpen kun je ze het beste even naar een string casten, door `string(var)` te gebruiken, of nog beter, de Go idioom `fmt.Printf("%s", var)`.

**Samenvattend; als iemand, wie dan ook, een bestand of tekst versleuteld met jouw publieke sleutel, ben jij de enige die het kan lezen, door het te ontcijferen met je privésleutel.**

Het versleutelen van een boodschap is de voornaamste functionaliteit van encryptie zoals RSA. Je kan er echter nog meer mee, zoals de afzender bewijzen. Daarvoor hoeft je eigen tekst niet eens encrypted te zijn. Dit doe je met de methoden Sign en Verify, waarbij je een boodschap &#8220;ondertekend&#8221; met je privésleutel waarna iedereen kan verifieren dat het echt jouw boodschap is door de Verify method.

Het ondertekenen van Henk&#8217;s publieke boodschap <span style="background-color: #e9ebec; color: #222222; font-family: Inconsolata, monospace;">msg</span>, _&#8220;wetenschap is ook maar een mening&#8221;,_ gebeurd met de private key van de boodschapper. De `[]byte` return value is de _signature_ die met de boodschap meegezonden moet worden.

{{< gist Grrrben 468ae117aead53847754fd2bf2261f2a "rsa_sign.go" >}}

Hierna kan _iedereen_ verifieren dat de boodschap inderdaad van Henk is, door de `Verify` method met Henk&#8217;s publieke sleutel aan te roepen:

{{< gist Grrrben 468ae117aead53847754fd2bf2261f2a "rsa_verify.go" >}}

Zodra er iets veranderd aan de boodschap `msg`, de signature `sig` of de publieke sleutel `pk` zal Verify een error terug geven. Dit kunnen we allemaal afvangen met een unittest:

{{< gist Grrrben 92138ee80ed66ae4bfd5a3b3956b7891 "test_sign_verify.go" >}}

Nog ééntje van Ingrid, om het af te sluiten:

> Misschien klopt het niet allemaal, maar het is wel waar.

Zie je iets aan de code dat je zelf anders zou doen? Schiet een PR in bij de [repository][1].<figure id="attachment_411" style="width: 733px" class="wp-caption alignnone">

[<img class="wp-image-411 size-full" src="https://atog.nl/wp-content/uploads/2018/11/public_key.png" alt="" width="733" height="282" srcset="https://atog.nl/wp-content/uploads/2018/11/public_key.png 733w, https://atog.nl/wp-content/uploads/2018/11/public_key-300x115.png 300w" sizes="(max-width: 733px) 100vw, 733px" />][2]<figcaption class="wp-caption-text">https://xkcd.com/1553/</figcaption></figure> 

Bronnen:

Golang RSA package; <https://golang.org/pkg/crypto/rsa/>

Github repository; https://github.com/Grrrben/golang-rsa

De quotes van Henk en Ingrid zijn geleend; <https://bronsdom.wordpress.com/2011/11/17/uitspraken-van-henk-en-ingrid/>

<p id="title" class="graf graf--h2 graf--leading">
  Explaining public-key cryptography to non-geeks; https://blog.vrypan.net/2013/08/28/public-key-cryptography-for-non-geeks/
</p>

 [1]: https://github.com/Grrrben/golang-rsa
 [2]: https://atog.nl/wp-content/uploads/2018/11/public_key.png