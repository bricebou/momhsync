---
title: 'Sublime Text & Markdown'
media_order: sublimemarkdownlivepreview.png
date: '08-11-2020 11:55'
taxonomy:
    category:
        - Linux
        - Windows
    tag:
        - 'Sublime Text'
content:
    items: '- ''@self.children'''
    limit: '5'
    order:
        by: date
        dir: desc
    pagination: '1'
    url_taxonomy_filters: '1'
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

## Packages

- [MarkdownEditing](https://github.com/SublimeText-Markdown/MarkdownEditing)&nbsp;: prend pleinement en charge le langage Markdown, fournit un ensemble de commandes utiles à la rédaction via la Command Palette...
- [TableEditor](https://github.com/vkocubinsky/SublimeTableEditor)&nbsp;: non spécifique au Markdown mais permet de faciliter la création de tableaux et d'en faciliter la lecture via un mécanisme d'alignement performant&nbsp;;
- [MarkdownPreview](https://github.com/facelessuser/MarkdownPreview)&nbsp;: permet de générer des aperçus au format HTML des documents en Markdown, soit via la commande `Preview in Browser` soit via la commande `Build` appelable avec le raccourci `Ctrl+b`&nbsp;;
- [MarkdownLivePreview](https://math2001.github.io/MarkdownLivePreview/)&nbsp;: scinde une fenêtre de Sublime Text et fournit un aperçu en direct de ce qui est saisi.
![Sublime MarkdownLivePreview](sublimemarkdownlivepreview.png)
- [Pandoc](https://github.com/tbfisher/sublimetext-Pandoc)&nbsp;: ce plugin permet de générer, à partir d'un fichier Markdown (entre autres formats), des documents PDF, Word, HTML... Il nécessite cependant de bien avoir le paquet `pandoc` d'installé&nbsp;:     
```shell
sudo apt install pandoc
```

## Snippets et raccourcis clavier

- guillemets français en HTML&nbsp;:    
```xml
<snippet>
    <content><![CDATA[
«&#8239;${1}&#8239;»$0
]]></content>
    <tabTrigger>og</tabTrigger>
    <scope>text.html.markdown,text.html</scope>
</snippet>
```
ou via le raccourci clavier `Alt+"`&nbsp;:
```json
    { "keys": ["alt+\""], "command": "insert_snippet", "args": {"contents": "«&#8239;${1}&#8239;»$0"}, "context":
        [
            { "key": "selector", "operator": "equal", "operand": "text.html.markdown,text.html" }
        ]
    },
```
- espace insécable en HTML avec le raccourci clavier `Ctrl+Espace`&nbsp;:
```json
	{ "keys": ["ctrl+space"], "command": "insert_snippet", "args": {"contents": "&nbsp;$0"}, "context":
        [
            { "key": "selector", "operator": "equal", "operand": "text.html.markdown,text.html" }
        ]
    },
```

## Configuration

Voici le contenu du fichier `~/.config/sublime-text-3/Packages/User/Markdown.sublime-settings`&nbsp;:

```json
{
	"color_scheme": "Packages/Monokai++/themes/Monokai++.tmTheme",
	"line_numbers": true,
	"highlight_line": true,
	"enable_table_editor": true,
}
```