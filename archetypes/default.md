---
title: "{{ substr (replace .Name "-" " " | title) 5 500 }}"
date: "{{ .Date }}"
thumbnail: "/img/{{ .Name }}.png"
categories: []
tags: []
url: "{{ substr .Name 5 500 }}"
---
