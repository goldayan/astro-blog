---
setup: |
  import Layout from '../../layouts/BlogPostLayout.astro';
title: COC SourceKit Contribution
description: Open Source contribution
date: 2020-07-03
author: "Gold Ayan"
category: "Coding"
tags:
  - open-source
---

This article about the story of my 3rd OpenSource contribution. How it all happened ?

I started using Neovim editor lot now a days so I want the editor to have ide like functionality(auto completion,jump to definition) for few programming languages like swift,python.

so i use [deoplete](https://github.com/Shougo/deoplete.nvim) for few weeks then i hear about [COC(Conquer Of Completion)](https://github.com/neoclide/coc.nvim) plugin which has completion for lot of languages so switched to it. To have ide like feasibility you need the particular language coc plugin
For python - coc-python plugin
For Swift - coc-sourceKit plugin
[More Plugin on here](https://github.com/neoclide/coc.nvim/wiki/Using-coc-extensions)

<!-- more -->

#### COC-SourceKit

- Sourcekit language server extension for coc.
- Coc sourcekit interacts with sourcekit-lsp which comes by default along with Xcode 11.4+.

I have lot of hope that i would be able to make neovim as my programming environment but turns out when i open a sample file from project it throw an error at,

```
import UIKit
```

It is main library used to develop apps, so i checked for any other keys,need to change inorder it to work but found a workaround in visual studio code plugin.More details on the workaround in [https://funnyitworkedlasttime.dev/posts/2020-01-10-swift-vscode-sourcekit-lsp/](https://funnyitworkedlasttime.dev/posts/2020-01-10-swift-vscode-sourcekit-lsp/)

Now I need to find a way to make it work in coc-sourcekit turns out coc can use visual studio plugins with slight modification. If i can make the change to the coc-sourcekit then I'll be able to see completion inside Neovim so opened a issue in the coc-sourcekit repository with the workaround as reference so the author of the repository can fix it.

![issue](/image/coc-sourcekit/issue.png)

It turns sideways. Wait a minute, Whaaaaaaat? I thought he will fix it.

I checked the repository, the extenstion is written in **Typescript** i never worked in that language. Damn how am i going to make this work.

I decided to give it a try whether i can able to solve this, After dozens of google search and reading the work around article couple of times. I managed to change the code in coc-sourcekit, Now its time to find the truth let's see whether it works

The changes are we are going to pass two extra values to sourcekit-lsp so it can able to find UIKit library.

1. ios Sdk path which can be obtained from **xcrun --sdk iphonesimulator --show-sdk-path**
2. Target architecture in the format of **x86_64-apple-ios13.2-simulator**.

I followed the readme of the coc-sourcekit to build and link it then open sample file from the project. It didn't work... ohhhhMG.

Checked the article again if i missed anything. I found it used project generated using swift package but i used project generated from xcode. Let's give it a try and when i started typing the auto completion appeared ( It worked, coooool).

![demo](/image/coc-sourcekit/demo.png)

Investigation begin on why it didn't work xcode generated project turns out it is not support by apple yet. I thought i can use this in my daily workflow but it only supports project generated using swift packages. So sad... :(

Any how i managed to make it work on coc-sourcekit so decided clean the code little bit and readme then send a Pull Request.

![pull request](/image/coc-sourcekit/pull_request.png)

Which got approved on 3-July and merged with master branch.
