---
title: "Bootstrap"
author: "zidenis"
date: 2022-08-21T15:57:04-03:00
draft: false
tags: ["blog"]
categories: []
keywords: ["bootstrap", "blog", "hugo", "github"]
menu: "main"
---

* Bootstrapping the blog. Using Hugo + hugo-theme-bootstrap4-blog.

```sh
$ wget https://github.com/gohugoio/hugo/releases/download/v0.101.0/hugo_0.101.0_Linux-64bit.tar.gz

$ tar xzf hugo_0.101.0_Linux-64bit.tar.gz 

$ cp hugo /usr/local/bin

$ hugo version
hugo v0.101.0-466fa43c16709b4483689930a4f9ac8add5c9f66 linux/amd64 BuildDate=2022-06-16T07:09:16Z VendorInfo=gohugoio

$ hugo new site zidenis

$ cd zidenis

$ git init

$ git submodule add https://github.com/zidenis/hugo-theme-bootstrap4-blog.git themes/bootstrap4-blog

$ git submodule add https://github.com/zidenis/zidenis.github.io.git

$ cat > config.toml << EOF
baseURL = "https://zidenis.github.io"
languageCode = "pt-br"
title = "zidenis blog"
theme = "bootstrap4-blog"
publishdir = "zidenis.github.io"
EOF

$ hugo new posts/bootstrap.md

$ hugo server -D

$ hugo -D
```