---
date: '{{ .Date }}'
title: '{{ replace .File.ContentBaseName "-" " " | title }}'
draft: false
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---
