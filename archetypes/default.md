---
date: '{{ .Date }}'
draft: true
title: '{{ replace .File.ContentBaseName "-" " " | title }}'
cover:
    image: https://placecats.com/300/200
    alt: "New alt"
    caption: "New caption"
    hidden: false
    hiddenInSingle: false
summary: "Insert summary here"
ShowReadingTime: false
tags: ["New project"]
---