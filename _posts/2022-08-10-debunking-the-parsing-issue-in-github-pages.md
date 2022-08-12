---
title: "Master of Markdown : Debunking the Parsing Issue in GitHub Pages"
date: YYYY-MM-DD HH:MM:SS +0600
author: 01
img_path: /assets/img/posts-img/2022-08-10/
image:
  path: /cover.jpg
  width: 500   # in pixels
  height: 500   # in pixels
  alt: A trojan horse meme about jekyll, kramdown, markdown and github pages 
categories: [WEB_DEVELOPMENT, GITHUB_PAGES]
tags: [jekyll, static-site, markdown, GFM, tech-tips, workaround]  
render_with_liquid: false

# img_path: /assets/img/posts-img/YYYY-MM-DD/
# Add image => ![Desktop View](/assets/img/sample/mockup.png){: w="700" h="400" }
# toc: false    // table of contents in right sidebar
# comments: false
# math: true    // math engine
# mermaid: true  // diagram tool
# pin: true
# > Example line for prompt.
# {: .prompt-info }     //  tip | info | warning | danger
# filepath highlight `/path/to/a/file.extend`{: .filepath}
# Hide Line Numbers: ```shell
#                   echo 'No more line numbers!'
#                   ```
#                   {: .nolineno }
#                   {: file="add-filename-here" }
---
![Web Dev](https://img.shields.io/badge/Domain-Web%20Dev-blue.svg)
![Topic: Tech Tips](https://img.shields.io/badge/Category-Tech%20Tips-green.svg)
![GitHub Pages](https://img.shields.io/badge/Environment-GitHub%20Pages-darkgreen.svg)
![Reading Time](https://img.shields.io/badge/Reading%20Time-4%20mins-white.svg)


## 📺 Context
 
* You keep a **GitHub** repository for documentation purpose.
* All the documents are written in [markdown](#markdown) syntax.
* You activate [GitHub Pages](#github-pages) to turn the repo into a nice little website (cause you're don't prefer reinventing the wheel).
* The site is published, and out of the blue, beautiful markdown text formattings are breaking up! 🤦‍♂️  
* Googling and going through _stack overflow_ threads and [GitHub Docs](https://docs.github.com/) aren't giving anything useful. 
* At this point, life ain't making sense anymore. ☹ But what if I tell you, there's a way? Take the red pill 💊, and see how deep the rabbit hole goes! 👇 

## ⏩ TL;DR

> In a rush? Skip to [The Fix](#📌-the-fix-🔨) section to get the solution.
{: .prompt-tip } 

## 🍦 Background

I'm an avid fan of cyber security engagements, and so I keep my solution writeups for [CTF (Capture The Flag)](#capture-the-flag) contests in my handy [ctf-journal](https://github.com/bijoy26/ctf-journal) repo.

This is how the `README.md` looks like in [VS Code](#vs-code) and GitHub web:
 
![GitHub View](gh-view.png){: w="500" h="500" }

After publishing through GitHub pages, the markdown-to-HTML conversion didn't seem to go the right way.

![Pages View](pages-view){: w="500px" h="500px" }

See the scaled out middle section? Clearly, GitHub's static site service is misinterpreting the markdown. Let's investigate and sort it out.

## 🔎 Solution

### **📌 Markdown Has Flavors?**

Back in 2004, **Markdown** came into being  with the intention of _being appealing to human readers in its markup form_. This made it widely popular in blogging, documentation, software development and readmes.

![Markdown](mark.png){: w="500px" h="500px" }
_Courtesy: `@developer_anand`_

But it had some ambiguities and inconsistencies, and so a lot of markdown flavors (syntax variations) started making their appearance. 

> 💡 To solve this issue, an uniform open source specification called [CommonMark](#commonmark) was born. 
{: .prompt-info } 

GitHub made their own dialect of markdown based on this spec by extending its features. This is called **GitHub Flavored Markdown**, often shortened as **GFM**. Now we know we are dealing with GFM whenever we use GitHub web.

> In case you're curious, check out the [list](https://github.com/commonmark/commonmark-spec/wiki/markdown-flavors) of flavors and a [comparison](https://gist.github.com/vimtaai/99f8c89e7d3d02a362117284684baa0f) between the commonmark compliant ones.
{: .prompt-tip } 

### **📌 Strange Case of Dr Jekyll**

When a repo is activated for the website service, GitHub uses [Jekyll](#jekyll) to build the site under the hood and then deploy into GitHub Pages. Jekyll is natively supported by GitHub, so the folder structure follows a generic Jekyll site layout.

![Jekyll](jekyll.png){: w="650px" h="650px" }

> The configuration of a Jekyll site entirely takes place in a `_config.yml` file on the root directoy. 
{: .prompt-info } 

It offers a lot of [configuration options](https://jekyllrb.com/docs/configuration/options/) for ease of development, integration and maintenance. 

Interestingly, there are options for markdown flavors as well. 🤔

### **📌 Connecting The Dots 🧵** 

All of the markdown variants are parsed by their corresponding  **markdown processor** for conversion into HTML format. GitHub Pages supports two such processors: 
1. Kramdown  
2. GitHub's own processor that renders GFM

**Kramdown** is the default Markdown renderer for Jekyll.
In this regard, GitHub claims-
> You can use GFM with either processor, but only our GFM processor will always match the results you see on GitHub.
{: .prompt-warning }

Cool! We found the stumbling brick. 😃 

Since GitHub was invoking Jekyll out-of-the-box with default options, my site's GFM markdown was being rendered by Kramdown and thus the HTML turned out different!

 
### **📌 The Fix 🔨** 

Following the [official docs](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/setting-a-markdown-processor-for-your-github-pages-site-using-jekyll), we can modify our repo's `_config.yml` file (create one if nonexistant) in the root directory. Add `markdown: GFM` option to override the kramdown parser.

![Fix](fix.png){: w="500px" h="500px" }

Trigger the build and deployment [workflow](#workflow) again and review the site once it is deployed.
 

![Pages Fixed](pages-fix.png){: w="500px" h="500px" }

All fixed! ✔ Faith in life restored! ❤

That's all for today folks.

---
## 🧲 Terminologies


1. #### GitHub Pages 
   A sweet static website service by **GitHub** that builds a site from the static contents of a repository. Such sites automatically bind to a `github.io` domain by default. 
1. #### Markdown
   A simple markup language for formatting texts and compiling into **HTML**.
1. #### Capture The Flag
   CTF is an **information security** competition which deals with forensics, cryptography, binary analysis, reverse engeneering and many more cyber security topics.
1. #### VS Code
   Visual Studio Code is a code editor by **Microsoft** for building and debugging modern web and cloud applications.
1. #### Jekyll
   A static site generator with a simplified build process involving markdown and HTML files.
1. #### Commonmark
    A rationalized specification of Markdown that defines the language syntax and offers a tests suite to validate Markdown implementations.
1. #### Workflow
   A configurable automated process that runs one or more jobs. The default `pages build and deployment` workflow by GitHub builds the sources and deploys it to GitHub pages.
---

## 📝 References  

- [GitHub Docs](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/setting-a-markdown-processor-for-your-github-pages-site-using-jekyll) 
- [Markdown Wiki](https://en.wikipedia.org/wiki/Markdown#:~:text=10%20External%20links-,History,the%20true%20structured%20text%20format%E2%80%9D.)
- [Jekyll Official Site](https://jekyllrb.com/)
- [GitHub Flavored Markdown](https://github.github.com/gfm/#what-is-github-flavored-markdown-)
- [Formal Specification For Markdown](https://www.smashingmagazine.com/2020/12/commonmark-formal-specification-markdown/)

---