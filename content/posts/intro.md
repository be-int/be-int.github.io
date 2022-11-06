---
title: "Publication d'un site utilisant Hugo & GitHuB (Pages) sur MacOS (Mx)"
date: 2022-11-06T11:00:26+01:00
draft: false
tags: ["MacOS", "Hugo", "GitHub"]
cover:
    image: img/hugo-logo-wide.svg
    alt: https://gohugo.io/
---

Ce site utilise Hugo (PaperMod) & GitHuB (Pages). Ci-dessous le tuto pour sa mise en place.

## Prérequis : installation de HomeBrew

Dans Terminal, entrer les lignes suivantes :
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

#replace xxx par votre user
echo '# Set PATH, MANPATH, etc., for Homebrew.' >> /Users/xxx/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/xxx/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"  

brew analytics off # https://docs.brew.sh/Analytics - pour empêcher l'envoi de données
```

## GitHub :

Créer un repository :

Nom : VotreUserGithub.github.io
Type : Publique
Cocher README file
Licence : MIT ou au choix ;)

## Hugo :
Dans Terminal
```
cd desktop
git clone https://github.com/VotreUserGithub/VotreUserGithub.github.io.git
cd VotreUserGithub.github.io
git checkout -b setup-my-blog
hugo new site --force . -f -yml
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

Edition du fichier config.yml 

```
nano config.yml
```
Un exemple de ce que vous pouvez y mettre
```
baseURL: "https://VotreUserGithub.github.io/"
languageCode: "en-us"
title: "BEiNT"

theme: "PaperMod"

enableRobotsTXT: true
buildDrafts: false
buildFuture: false
buildExpired: false

minify:
  disableXML: true
  minifyOutput: true

params:
  defaultTheme: light

  cover:
    linkFullImages: true

  ShowBreadCrumbs: true
  ShowCodeCopyButtons: true

  ShowReadingTime: true

  assets:
    favicon: "favicon.ico"

menu:
  main:
    - identifier: archives
      name: Archives
      url: archives/
      weight: 2
    - identifier: tags
      name: '#'
      url: tags/
      weight: 3
    - identifier: search
      name: Search
      url: search/
      weight: 1
  
outputs:
    home:
        - HTML
        - RSS
        - JSON
```
ctrl + o pour sauver  
ctrl + x pour quitter
