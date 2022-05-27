---
setup: |
  import Layout from '../../layouts/BlogPostLayout.astro';
title: Handling Knowledge
description: This is an example blog post
date: 2019-12-15
author: "Gold Ayan"
category: "Personal"
tags:
  - markdown
  - org
  - latex
  - notes
---

Everything becomes digital nowdays. I barely ever touch paper and pen so how to
create and manage notes?

One important thing, I want my notes to be able to create/Edit using simple text editor
such as Notepad,TexEdit... not fancy software like MS-Word.

<!-- more -->

Few solution i am using it in my daily life.let me share with you and
pick one that excits you.

you just need a text editor to get started But i have mentioned other tools which
will assit you in managing or providing extra functionalities like auto completion,
colours and shortcuts.

## Markdown

- Many popular note taking app like Evernote supports markdown and learning
  markdown only take you 2-10 minutes.It's just that simple.
- You can also write a entire book in markdown and convert to .epub or .pdf
- This article is written in markdown which is compiled using static site generator
  (zola in my case) to html
- You can maintain todo,insert image,tables.
- **pandoc**( command line tool ) can able to convert your markdown files to other
  file formats such as pdf,html..

### Sample code

```md
# An h1 header

Paragraphs are separated by a blank line.

_Italic_, **bold**, and `monospace`.

Undordered List

- one
- two
- three
```

### Resources

- [https://www.markdownguide.org/getting-started](https://www.markdownguide.org/getting-started)
- [https://www.markdowntutorial.com](https://www.markdowntutorial.com)

### Managing Tools

- [Zettlr](https://www.zettlr.com/) free software that can help you manage
  markdown files.
- [Notion](https://www.notion.so/) paid software but you can see how markdown
  can be used in different tasks.
- vim + [vimwiki](https://github.com/vimwiki/vimwiki) plugin for vim can able to link
  to different markdown file. It will help you to build own wikipedia.

choose one which suits you.

## Latex

Markdown is great but sometimes you need more.I first heard the term when i
attend FStival event in Thagaraja college about linux software. I was jawdroped
by the functionalities of latex.
One best example is Math, thing about all those equations, derviation
awwwwwww how pain it will be to write.

say hello to latex

using latex you can write

- Books ( maths book )
- Scienticfic paper,
- Thesis

It has the capability more than Ms-word but you have to code. I never used much
but definetly worth learning.

### Sample code

```tex
\documentclass{article}

\begin{document}
First document. This is a simple example, with no
extra parameters or packages included.
\end{document}
```

### Resources

- [https://www.overleaf.com/learn/latex/Learn_LaTeX_in_30_minutes](https://www.overleaf.com/learn/latex/Learn_LaTeX_in_30_minutes)
- [https://www.latex-tutorial.com/tutorials](https://www.latex-tutorial.com/tutorials)
- [Interactive LaTeX Editor](https://arachnoid.com/latex/)

### Tools

- Lyx
- TeXstudio
- Pandoc
  - you can write latex in text editor then use pandoc to convert to pdf.
  - below link will provide you necessary details to get started
  - [https://pandoc.org/installing.html](https://pandoc.org/installing.html)

## Org

Org file is more awesome but only problem with org you need install emacs editor
inorder to use it with functionalities such as shortcuts to create headers,
bullets,check and uncheck list.

If you are conformtable with emacs then org can change your file completely
i use emacs with evil mode ( vi key binding in emacs ) for org.

Best thing is you can do is writing latex inside org.

most powerful feature in org mode is **Literate programming** which is writing
code and notes in same file but you can able to compile code without having to
comment the code similar to jupyter notebook.Code are written between
tag **_#+BEGIN_SRC_** and **_#+END_SRC_**, the result are displayed in the
**_#+RESULTS_**

Other notable things are **agenda**( use it as a scheduler ) and **TODO**.

### Sample code

```
#+BEGIN_SRC python3

print("Hello Readers")

#+END_SRC
```

check this link: [https://raw.githubusercontent.com/ThangaAyyanar/elisp-guide/master/README.org](https://raw.githubusercontent.com/ThangaAyyanar/elisp-guide/master/README.org) for more context.

### Resources

- [orgmode.org](http://orgmode.org)

### Tools

- Emacs Editor: It is bit different from normal text editor.few configuration
  can give you org editing editor.
- Online editors - [https://org-web.org](https://org-web.org)
- Android - orgzly

### Managing Tools

- [org-brain](https://github.com/Kungsgeten/org-brain)
- [org-wiki](https://github.com/caiorss/org-wiki) similar to vim-wiki

## Graphviz

This program is bit different from above.

create graph from code and you can customize the graph too. it is used by
compilers, decompilers, graph software to make graph

It uses **dot** language

### Sample code

```
digraph {
  node [ shape=square ];
  edge [ style=dashed ];

  a -> b -> c;
}
```

### Resources

- [http://www.graphviz.org](http://www.graphviz.org)

### Tools

- graphviz program
- sketchviz
  - It is interactive and a best way to learn graphviz

## Plantuml

This program convert code to UML diagrams

> UML - Unified Modeling Language

If you are a computer science student you definetly heard UML diagrams which is representing
logic in some kind of diagrams. such as

- Visualize the flow of code.
- Abstraction of the software.
- Time diagram.
- Sequence diagram.

you can also able to create **Mindmap** , wireframes and more

### Sample code

```
@startuml
Class01 <|-- Class02
Class03 *-- Class04
Class05 o-- Class06
Class07 .. Class08
Class09 -- Class10
@enduml
```

### Resources

- [https://plantuml.com](https://plantuml.com)

### Tools

- Download the jar file from [Plant uml website](https://plantuml.com/download)

## Reveal.js or Impress.js

The biggest problem in doing presentation is you need to have MS Powerpoint or
some kind of third party software but for this software you only need web browser.

I was using linux at that time of college so ppt done in open office, Libre office
usually break when opened in MS-Powerpoint so i searched for alternate software i
found this two goodies.

you may guessed from the header of the topic it is related to javascript.

you can create presentation from using html files such by importing this frameworks
i used to do my presentation using this tools. it takes few minutes to learn and
let you able to create basic presentation in minutes.

But something special we can do this technology

consider the scenario you have ton of notes and want to change it into
presentation

Remember our friends from above

- markdown
- org file
  you can convert them to presentation using tools which comes prety handy feature
  if your a student.

### Tools

- For Markdown - [reveal-md](https://github.com/webpro/reveal-md)
- latex - [Beamer](https://www.overleaf.com/learn/latex/Beamer)
  ( It doesnot use reveal but we can create presentation from latex )
- Org - [org-reveal](https://github.com/yjwen/org-reveal)

Check out the demo presentation:

- impress.js -> [https://impress.js.org](https://impress.js.org)
- reveal.js -> [https://revealjs.com](https://revealjs.com)

Though, I write in html didn't know the potential of mardown and org when i was
student this is will be good addition to the list.

## Final Touch

I use mostly markdown and few org for my notes and it is easy for me create and
maintain using version control system like github.

I know that's a lot of tools, check it in your free time check whether it can solve
any of your problem.

Let me know which software do you use to manage your notes in comments.
