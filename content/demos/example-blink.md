---
title: "Example: blink"
date: 2026-11-26
draft: true   # a layout check, not for publishing: `hugo server -D` shows it
description: "Layout check for the demos section, using the landing page's hero clip."
boards: ["esp32-c3"]
tags: ["example"]
---

{{< video src="/blink-demo" poster="/blink-demo-poster.jpg" loop="true" caption="The landing page's hero clip, played through the `video` shortcode." >}}

This draft exists to check the layout: a looping clip, the board and tag chips, and the prev/next links.
Delete it once there's a real demo.

```python
# A code block, to check highlighting in light and dark.
from machine import Pin
led = Pin(8, Pin.OUT)
```
