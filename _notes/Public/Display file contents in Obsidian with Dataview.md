---
title: Display file contents in Obsidian with Dataview
notetype: feed
date: 2025-09-14
---

Obsidian released [Bases](https://help.obsidian.md/bases),
which covers like 90% of the use cases I had for [Dataview](https://blacksmithgu.github.io/obsidian-dataview/).
I still use it for a very specific scenario, though.

Sometimes, I want to display a list of (very short) files, but I also want to display their contents.
There isn't a straightforward way to do this with Dataview and its Dataview Query Language,
but you can achieve this by using its JavaScript API. Here's how I do that:

```javascript
let journals = dv.pages('#journal AND #work');

let headers = ["File", "Content"];
let elements = [];
for (let journal of journals.sort(journal => journal.file.name, "desc")) {
  let content = await dv.io.load(journal.file.path);
  elements.push([journal.file.link, content]);
}

dv.table(headers, elements);
```
