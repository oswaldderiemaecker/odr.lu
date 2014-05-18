---
layout: post
title:  "MYSQL: utf8_general_ci vs utf8_unicode_ci"
date:   2012-05-14 21:47:09
categories: mysql
---
Both utf8_general_ci and utf8_unicode_ci perform **case-insensitive** comparison (in constrast of utf8_bin which is **case-sensitive** because it compares the binary values of the characters).

By comparisons for the utf8_general_ci collation are faster, but slightly less correct, than comparisons for utf8_unicode_ci.

The reason for this is that utf8_unicode_ci supports mappings such as expansions; that is, when one character compares as equal to combinations of other characters.

For example, in German and some other languages “ß” is equal to “ss”. utf8_unicode_ci also supports contractions and ignorable characters. utf8_general_ci is a legacy collation that does not support expansions, contractions, or ignorable characters. It can make only one-to-one comparisons between characters.

When you need better sorting order use **utf8_unicode_ci**, 
and when you are interested in performance **use utf8_general_ci**.

