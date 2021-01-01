---
title: "Tmux"
date: 2020-01-27T20:17:00+01:00
draft: true
type: post
---
tmux is een multiplexer, een programma dat je meerdere terminals in een terminal kan laten draaien. 
Ik gebruik het vaak om dingen te groeperen. 
Een docker project bijvoorbeeld, wat op een deel van je terminal scherm draait, 
en op een andere deel een de mysql cli app, of de project root voor git commando's. 
Alles in een cluster op het scherm. 

Out-of-the-box is tmux al een prima programma. 
Maar toch heb ik altijd een paar wijzigingen in de configuratie:

```
# $ cat ~/.tmux.conf 

# set Ctrl-a as prefix, same as screen
set -g prefix C-a
unbind-key C-b
bind-key C-a send-prefix

# switch panes using Alt-arrow without prefix
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D

# Enable mouse control (clickable windows, panes, resizable panes)
set -g mouse on
```

Door de muis control aan te zetten verlies je wel de standaard selectie mogelijkheid van de muis. Hiervoor moet je nu `shift+mouse` gebruiken.

Knip/plak binnen tmux.

1. Copy mode begin je met `Ctrl+a [` (a is de prefix..!)
2. Ga naar je begin bestemming en begin met selectie door op `Ctrl-spatie` te drukken.
3. Selecteer de gewenste tekst.
4. `Alt+w` kopieert de tekst naar de tmux buffer
5. Plakken gaat via `Ctrl+a ]`.


