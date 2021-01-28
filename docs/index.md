--8<-- "includes/abbreviations.md"

# Developer Docs STAT4.0

##1. General
###1.1. Principle
specify the future, don't document the past.

###1.2. Process
iterative as part of ongoing change of whatever (software development, business process, user journey): Ideate, Specify, Implement, Rollout, Measure.

###1.3. Questions / Decisions
* handled in slices or general or both?
* in each bundle or general doc repo (github.com/statista/docs-developers)?
* do we use that for external docs as well (e.g. api)?

##2. Writing Documentation
#### [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
#### Installed Extras and usage
### Abbreviations
abbreviations can be defined with a special syntax similar to URLs and footnotes at any point in the Markdown document.
```    
The HTML specification is maintained by the W3C.

*[HTML]: Hyper Text Markup Language
*[W3C]: World Wide Web Consortium
```

### Glossary
abbreviations can also get included from a central file – a glossary – and embed them into any other file.

_Result:_

The HTML specification is maintained by the W3C.
```    
--8<-- "includes/abbreviations.md"
```

### Smart Symbols

Automatically displays readable symbols(tm), see [official documentation](https://facelessuser.github.io/pymdown-extensions/extensions/smartsymbols/)