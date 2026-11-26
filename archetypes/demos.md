---
title: "{{ replace .File.ContentBaseName "-" " " | title }}"
date: {{ .Date }}
draft: true
description: ""
boards: []
tags: []
---

{{ "{{<" }} video src="/media/{{ .File.ContentBaseName }}" poster="/media/{{ .File.ContentBaseName }}-poster.jpg" loop="true" {{ ">}}" }}

What it shows, in a few lines. Link the flow file and the docs page it relies on.
