# How to build this site

## prepare environment

```
pip install --upgrade pip
pip install mkdocs
pip install mkdocs-material
mkdocs new my-project
mkdocs serve

```

## modify mkdocs.yml

```
site_name: classifieds
site_url: https://github.corp.ebay.com/clsfd/Document
site_description: 'Blog for CLSFD ATEAM'
site_author: 'Xin, Gen'
google_analytics:
  - 'UA-127044749-1'
  - 'auto'
nav:
- Home:
  - Index: index.md
  - How to contribute: help.md
- Project:
  - This is 0: project/about.md
- Batch Support:
  - 2018:
    - Sep:
      - 09: batch_support/batch_example.md
- Share:
  - 2018:
    - Sep:
      - unicode: share/unicode.md
      - ssh: share/ssh.md
      - test: share/mytest.md
- Organization:
  - Memebers: organization/our.md

extra_javascript:
  - 'https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-MML-AM_CHTML'

theme:
  name: 'material'
  palette:
    primary: 'indigo'
    accent: 'indigo'
  font:
    text: 'Roboto'
    code: 'Roboto Mono'
  feature:
    tabs: true
  logo: img/favicon.ico
markdown_extensions:
  - admonition
  - codehilite
  - footnotes
  - pymdownx.arithmatex
  - pymdownx.betterem:
      smart_enable: all
  - pymdownx.caret
  - pymdownx.critic
  - pymdownx.details
  - pymdownx.emoji:
      emoji_generator: !!python/name:pymdownx.emoji.to_svg
  - pymdownx.inlinehilite
  - pymdownx.magiclink
  - pymdownx.mark
  - pymdownx.smartsymbols
  - pymdownx.superfences
  - pymdownx.tasklist:
      custom_checkbox: true
  - pymdownx.tilde
```

## deploy to Github Pages

1. create empty repository. Here we use Document as one example.
2. git remote add origin git@github.corp.ebay.com:clsfd/Document.git
3. git push -u origin master
4. mkdocs gh-deploy


## How to modify/add pages
1. git clone git@github.corp.ebay.com:clsfd/Document.git
2. modify/add markdown files
3. add this file to mkdocs.yml
4. mkdocs serve
5. git commit -a -m "you commit content and reasons'
6. git push orign master
7. mkdocs gh-deploy

## Reference
1. [Mkdocs](https://www.mkdocs.org/)
2. [Meterial for Mkdocs](https://squidfunk.github.io/mkdocs-material/)
3. [Github Pages](https://squidfunk.github.io/mkdocs-material/)

