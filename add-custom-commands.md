---
layout: default
title: Add custom commands
nav_order: 1
---

# Blutilities

* Comming soon *

# C++ functions

Any C++ exposed `UFUNCTION` can be marked with the `QuickActionEntry` meta tag and it will be automatically picked up by the quick menu.

Example usage:
```c++
// Example tooltip
UFUNCTION(meta = (QuickActionEntry))
void ExampleFunction();
```

By using this method, you can add entries to the quick menu *without* adding a hard depedency on the plugin. This way your code will still work even after the plugin is removed *with no additional changes*.

{: .warning }
Currently only functions with no input arguments are supported

# Custom

