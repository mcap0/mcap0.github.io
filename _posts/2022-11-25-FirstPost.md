---
title: Sharing Automatico di Markdown Files su GitHub Pages
date: 2022-11-23 12:00:00 -500
categories: [Security Research]
tags: [gitub,automation,sharing,jekyll,chirpy]
---
## Perché ti serve un sistema di notes sharing?

E' uso comune tra gli informatici avere una quantita di note e appunti sparsi tra molteplici applicazioni e interessi vari. Queste note molto spesso vengono lasciate al loro destino, e in effetti il loro primario utilizzo è proprio un organizzazione mentale per chi le crea, in modo da permetterci di comprendere meglio un argomento o avere sempre a portata di mano una lista personalizzata di funzioni utili ma difficili da ricordare.

Alcuni sceglieranno di pubblicare le proprie risorse su GitHub per potervi accedere sempre e da ovunque, altri invece per una motivazione più nobile e antica: aiutare le persone che incorrono in un problema che tu hai già risolto.

## GitHub Pages & Jekyll

Github Pages permette agli utenti di creare siti web ospitati da Github. I siti web sono creati usando i file di codice e i contenuti dei repository di Github. Gli utenti possono scegliere di rendere pubblici o privati ​​i propri siti web.

Il tool Jekyll per GitHub Pages consente di creare e gestire facilmente i propri siti web statici. Jekyll consente di generare automaticamente il codice HTML del sito web, in base al contenuto scritto in un semplice formato di testo, come ad esempio Markdown o Liquid. Inoltre, Jekyll è in grado di generare automaticamente RSS feed e file di mappa del sito (sitemap.xml), in modo da rendere il sito web più facilmente scansionabile dai motori di ricerca.

Per poter utilizzare Jekyll per GitHub Pages, è sufficiente creare un nuovo repository su GitHub e seguire la semplice procedura descritta sul sito web di Jekyll. In pochi minuti, sarà possibile creare un sito web statico professionale, utilizzando uno dei tanti temi disponibili per Jekyll.

## Installation & Tips

Per qualsiasi tema tu scelga di utilizzare con Jekyll, dovrai prima completare l'installazione del tool nel tuo computer. Quest'ultima avviene con Ruby Gems ed il processo varia a seconda del tuo sistema operativo. [Qui](https://jekyllrb.com/docs/installation/) la documentazione ufficiale.

In particolare il tema che ho scelto di applicare si chiama [Chirpy](https://chirpy.cotes.page/posts/getting-started/).

Per la configurazione di Chirpy ho seguito un [video tutorial](https://youtu.be/F8iOU1ci19Q) e la [documentazione ufficiale](https://chirpy.cotes.page)

### Pros
- Completamente Gratis e Open Source
- Markdown è uno formato molto utilizzato
- GitHub Actions ti permette di aggiornare il sito web automaticamente ad ogni push

## Bash Script per l'automazione

Il seguente è uno script che uso per pubblicare automaticamente dei file markdown. 
Da migliorare con: 
- Bash profile alias, 
- Scelta [categories] automatica e non predefinita 

```bash
#!/bin/bash

path=$1

original=$path

echo "$original"

renamed=$(date +%Y-%m-%d)-$(echo "${path// /_}")

echo "$renamed" 

touch $renamed

echo "---
title: $original
date: $(date +%Y-%m-%d) 12:00:00 -500
categories: [Tech]
tags: [tech]
--- 
" > $renamed

cat "$original" >> $renamed

cp $renamed ~/mcap0.github.io/_posts/

cd ~/mcap0.github.io

git add .

git commit -a --allow-empty-message -m ''

git push

echo "Done."
```