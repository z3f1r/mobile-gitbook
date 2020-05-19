---
description: >-
  can create universal binaries from Mach-O files, extract regular object files
  from universal binaries, and display architecture information about both
  universal and regular files.
---

# lipo

man: [https://llvm.org/docs/CommandGuide/llvm-lipo.html](https://llvm.org/docs/CommandGuide/llvm-lipo.html)

пример:

```text
lipo -archs <binary path>
```

 С помощью file можно проверить, что это mach-O: 

```text
file <binary> | grep Mach-O
```

