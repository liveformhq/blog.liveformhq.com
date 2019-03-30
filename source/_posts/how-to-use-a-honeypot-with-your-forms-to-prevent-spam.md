---
title: How to use a honeypot with your forms to prevent spam
date: 2019-03-29 20:40:01
tags:
- Honeypot
- Spam
---

LiveForm now allows you to use [Honeypot's](https://en.wikipedia.org/wiki/Honeypot_(computing)) in your form.

All you need to use a honeypot is add a hidden field with a name `trap`.

```html
<form ... >
  <....
  <input type=text style='display: none' name=_trap />
  <....
</form>
```

Whenever your forms get submitted with this field filled in, we'll mark them as
spam.
