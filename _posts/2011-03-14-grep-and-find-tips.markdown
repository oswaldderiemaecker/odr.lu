---
layout: post
title:  "grep & find tips"
date:   2011-03-21 21:47:09
categories: linux
---

Move all files containing a pattern in a folder

```bash
grep -r -l -Z 'test' . | xargs -0 -I{} mv {} ../folder/
```

Show files and date for a grep searched pattern:

```bash
grep -l 'test' * | xargs -Ifile ls -alih file
```

Show Lines before and after for a grep searched pattern:

```bash
grep -B1 -A2 -n port myfile
```

The above command will show not only each line containing the pattern “test” but it also print 1 line above it and the next two lines below it.
