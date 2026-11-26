---
title: "{{ replace .File.ContentBaseName "-" " " | title }}"
date: {{ .Date }}
draft: true
description: ""   # one sentence for lists, feeds and link previews; the first paragraph is used if empty
boards: []        # e.g. ["esp32-c3", "pico"]; only boards that have been tested
tags: []
# image: /media/this-post-og.png   # 1200x630 link-preview image; the site's og.png if left out
---
