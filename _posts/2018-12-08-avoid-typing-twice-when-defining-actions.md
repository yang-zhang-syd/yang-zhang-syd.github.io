---
layout: post
title:  "Avoid Typing Twice When Defining Action Names"
date:   2018-12-08 00:00:00 +1000
categories: tech blogs
---

Found a nice package which can help defining actions a lot easier by avoiding type the key/value twice when defining action names. 

The package is [KeyMirror](https://www.npmjs.com/package/keymirror). Simply import it into the source code and then you can build a dictionary with key/value pairs are of the same values.

```
var keyMirror = require('keyMirror');
var COLORS = keyMirror({blue: null, red: null});
var myColor = COLORS.blue;
var isColorValid = !!COLORS[myColor];
```

