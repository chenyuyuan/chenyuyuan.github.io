---
title: error-sub process /usr/bin/dpkg returned an error code (1)
date: 2019-12-07 18:41:55
tags: error
---

##### error:

```
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

##### environment: 

```
Ubuntu 16.04
```

##### solution:

```html
rm /var/lib/dpkg/info/$nomdupaquet* -f
```

