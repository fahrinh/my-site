---
title: "Static Blog dengan Hugo + Github Pages (+ TravisCI)"
date: 2017-07-22T21:02:09+07:00
categories: ["Drafts"]
tags: ["Hugo", "Github Pages", "Travis CI"]
draft: false
toc: true
---


## Github
Buat dua repositori di Github :

- Repo sumber konten (hasil dari `hugo new site`) : [github.com/fahrinh/my-site](github.com/fahrinh/my-site)
- Repo utama web pages (hasil generate `hugo`) : [github.com/fahrinh/fahrinh.github.io](github.com/fahrinh/fahrinh.github.io)

Buat Personal Access Token di https://github.com/settings/tokens dengan scope `repo`

Copy token tersebut ke clipboard.

## Travis CI
Buat file konfigurasi `.travis.yml` pada repo https://github.com/fahrinh/my-site/ dengan isi :
```
language: go

go:
  - master # This uses automatically the latest version of go

install:
  - go get github.com/spf13/hugo # This provides the latest version of Hugo to Travis CI

script:
  - hugo --theme=hugo-pacman-theme # This commands builds your website on travis

deploy:
  local_dir: public # Default static site output dir for Hugo
  repo: fahrinh/fahrinh.github.io # This is the slug of the repo you want to deploy your site to
  target_branch: master # GitHub pages branch to deploy to (in other cases it can be gh-pages)
  provider: pages
  skip_cleanup: true
  github_token: $GITHUB_TOKEN # This is the authentication which you will setup in the next step in travis-ci dashboard
  email: fahri.cyber@gmail.com
  name: "Fahri Nurul Hidayat"
  on:
    branch: master
```

Buat **Environment Variables** `GITHUB_TOKEN` di https://travis-ci.org/fahrinh/my-site/settings dengan value yaitu Personal Acces Token yang sudah dibuat sebelumnya.


Sumber: https://martinkaptein.github.io/blog/hugo-with-travis-ci-on-gh-pages/