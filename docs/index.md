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

The HTML specification is maintained by the W3C.

*[HTML]: Hyper Text Markup Language
*[W3C]: World Wide Web Consortium

```    
The HTML specification is maintained by the W3C.

*[HTML]: Hyper Text Markup Language
*[W3C]: World Wide Web Consortium
```

### Glossary
abbreviations can also get included from a central file – a glossary – and embed them into any other file.

_Result:_

The Content-Mixer is used in Statista's Queuing to enrich content data with user data. It consumes messages created by the Content MLS.
```    
--8<-- "includes/abbreviations.md"
```

### Smart Symbols

Automatically displays readable symbols(tm), see [official documentation](https://facelessuser.github.io/pymdown-extensions/extensions/smartsymbols/)

### Highlighting
- ==This was marked==
- ^^This was inserted^^
- ~~This was deleted~~

```
- ==This was marked==
- ^^This was inserted^^
- ~~This was deleted~~
```

Text can be {--deleted--} and replacement text {++added++}. This can also be
combined into {~~one~>a single~~} operation. {==Highlighting==} is also
possible {>>and comments can be added inline<<}.

```
Text can be {--deleted--} and replacement text {++added++}. This can also be
combined into {~~one~>a single~~} operation. {==Highlighting==} is also
possible {>>and comments can be added inline<<}.
```

### Notes

[More Information & Examples](https://squidfunk.github.io/mkdocs-material/reference/admonitions/)

#### Simple Example
!!! note
The path of the righteous man is beset on all sides by the inequities of the selfish and the tyranny of evil men. Blessed is he who, in the name of charity and good will, shepherds the weak through the valley of darkness, for he is truly his brother's keeper and the finder of lost children.

```
!!! note
The path of the righteous man is beset on all sides by the inequities of the selfish and the tyranny of evil men. Blessed is he who, in the name of charity and good will, shepherds the weak through the valley of darkness, for he is truly his brother's keeper and the finder of lost children.
```

#### Changing Title
!!! note "changed title"
The path of the righteous man is beset on all sides by the inequities of the selfish and the tyranny of evil men. Blessed is he who, in the name of charity and good will, shepherds the weak through the valley of darkness, for he is truly his brother's keeper and the finder of lost children.

```
!!! note "changed title"
The path of the righteous man is beset on all sides by the inequities of the selfish and the tyranny of evil men. Blessed is he who, in the name of charity and good will, shepherds the weak through the valley of darkness, for he is truly his brother's keeper and the finder of lost children.
```

#### Supported Types
##### abstract
```
abstract
```
!!! abstract "abstract"
The path of the righteous man is beset on all sides by the inequities of the selfish and the tyranny of evil men. Blessed is he who, in the name of charity and good will, shepherds the weak through the valley of darkness, for he is truly his brother's keeper and the finder of lost children.

##### warning
```
warning
```
!!! warning "warning"
The path of the righteous man is beset on all sides by the inequities of the selfish and the tyranny of evil men. Blessed is he who, in the name of charity and good will, shepherds the weak through the valley of darkness, for he is truly his brother's keeper and the finder of lost children.

##### info
```
info
```
!!! info "info"
The path of the righteous man is beset on all sides by the inequities of the selfish and the tyranny of evil men. Blessed is he who, in the name of charity and good will, shepherds the weak through the valley of darkness, for he is truly his brother's keeper and the finder of lost children.

##### tip
```
tip
```
!!! tip "tip"
The path of the righteous man is beset on all sides by the inequities of the selfish and the tyranny of evil men. Blessed is he who, in the name of charity and good will, shepherds the weak through the valley of darkness, for he is truly his brother's keeper and the finder of lost children.

##### success
```
success
```
!!! success "success"
The path of the righteous man is beset on all sides by the inequities of the selfish and the tyranny of evil men. Blessed is he who, in the name of charity and good will, shepherds the weak through the valley of darkness, for he is truly his brother's keeper and the finder of lost children.

##### bug
```
bug
```
!!! bug "bug"
The path of the righteous man is beset on all sides by the inequities of the selfish and the tyranny of evil men. Blessed is he who, in the name of charity and good will, shepherds the weak through the valley of darkness, for he is truly his brother's keeper and the finder of lost children.

##### quote
```
quote
```
!!! quote "quote"
The path of the righteous man is beset on all sides by the inequities of the selfish and the tyranny of evil men. Blessed is he who, in the name of charity and good will, shepherds the weak through the valley of darkness, for he is truly his brother's keeper and the finder of lost children.
