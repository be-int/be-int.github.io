---
title: "Publication d'un site utilisant Hugo & GitHuB (Pages) sur MacOS (Mx)"
date: 2022-11-06T11:00:26+01:00
draft: false
tags: ["MacOS", "Hugo", "GitHub"]
cover:
    image: img/hugo-logo-wide.svg
    alt: https://gohugo.io/
---

Ce présent site utilise Hugo (thème PaperMod) & GitHuB (Pages). Ci-dessous le tuto pour sa mise en place.

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

## Github Push :

```
git add .
git status
git commit -m "Commit initial"
git push origin setup-my-blog
```

Sur votre repo -> Compare & pull request -> Create pull request -> Merge pull request -> Confirm merge

```
git checkout main
git pull
```

## Deployement automatique de votre site

```
git checkout -b site_deploy
mkdir -p .github/workflows
touch .github/workflows/sd.yml
```
Editer le fichier sd.yml :
```
nano .github/workflows/sd.yml
```
Collez-y ce code pour déployer automatiquement votre site :
```
name: Build and Deploy Site

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build-and-deploy-site:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v2
        with:
          submodules: true
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'

      - name: Build site with Hugo
        run: hugo --minify

      - name: Check HTML
        uses: chabad360/htmlproofer@master
        with:
          directory: "./public"
          arguments: --only-4xx --check-favicon --check-html --assume-extension --empty-alt-ignore --disable-external
        continue-on-error: true

      - name: Deploy to GitHub Pages
        if: github.event_name == 'push' && github.ref == 'refs/heads/main'
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```
```
git add .github/workflows/sd.yml
git commit -m "Deployement automatique du site"
git push origin site_deploy
```
Sur votre repo -> Compare & pull request -> Create pull request -> Merge pull request -> Confirm merge

Dans les settings de votre repo -> Pages :
Select gh-pages & /(root) -> cliquez sur Save

Après une minute, vous pouvez aller consulter votre site sur : https://VotreUserGithub.github.io

```
git checkout main
git pull
```

### Bonus
Markdown & YAML cheat sheet : https://learn-the-web.algonquindesign.ca/topics/markdown-yaml-cheat-sheet/
