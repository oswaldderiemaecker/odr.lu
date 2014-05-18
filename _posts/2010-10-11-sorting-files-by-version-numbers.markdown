---
layout: post
title:  "Sorting by Version Numbers"
date:   2010-10-11 21:47:0                                                                                                                                                                                                                    
categories: linux
---

e.g assume you have a list of sql update files that need to be executed based on the version of folders and filenames.

1.1.1/11_sql1.sql

...

1.1.1/01_sql1.sql

1.1.0/01_sql1.sql

1.1.0/02_sql1.sql

1.2.0/01_sql1.sql

1.2.1/01_sql1.sql

1.2.1/02_sql1.sql

...

1.2.1/11_sql1.sql

To sort use:

``` bash
ls -d ./*/* | sort -V
```

-V, --version-sort
    natural sort of (version) numbers within text
