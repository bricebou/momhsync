---
title: 'Sublime Text & LaTeX'
date: '08-11-2020 11:53'
taxonomy:
    category:
        - LaTeX
        - Linux
    tag:
        - 'Sublime Text'
twitterenable: true
twittercardoptions: summary
facebookenable: true
content:
    items: '@self.children'
    limit: '5'
    order:
        by: date
        dir: desc
    pagination: '1'
    url_taxonomy_filters: '1'
---

## Packages

- [LaTeXTools](https://github.com/SublimeText/LaTeXTools)

## Snippets et raccourcis clavier

- guillemets français&nbsp;:
```xml
<snippet>
    <content><![CDATA[
\og ${1} \fg{}$0
]]></content>
    <!-- Optional: Set a tabTrigger to define how to trigger the snippet -->
    <tabTrigger>og</tabTrigger>
    <!-- Optional: Set a scope to limit where the snippet will trigger -->
    <scope>text.tex.latex</scope>
</snippet>
```
ou via le raccourci clavier `Alt+"`&nbsp;
```json
    { "keys": ["alt+\""], "command": "insert_snippet", "args": {"contents": "\\og ${1} \\fg{}$0"}, "context":
        [
            { "key": "selector", "operator": "equal", "operand": "text.tex.latex" }
        ]
    },
```

