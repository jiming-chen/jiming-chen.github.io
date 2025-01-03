---
date: '{{ .Date }}'
title: '{{ replace .File.ContentBaseName "-" " " | title }}'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---
