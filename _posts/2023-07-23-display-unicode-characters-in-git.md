---
layout: post
title: Display Unicode characters in git
date: 2023-07-23
categories: git
---

By default, git escapes “unusual” characters with backslashes and
their octal code. To stop this from happening, you can set
`core.quotepath` to `false`.

```shell
git config core.quotepath false
```

## Changelog

2023-07-23
: Page creation.
