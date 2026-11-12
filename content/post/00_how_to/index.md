---
title: How to write a blog post
description: This is the start of LaminarMEG blog journey
slug: 02_how_to
date: 2026-09-23 00:00:00+0000
image: cover.png
categories:
    - how_to
tags:
    - blog
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
links:
    - id: caiwebsiteguide
      title: Website template documentation
      website: https://stack.cai.im/guide/
---

Everything is in markdown so for all your text, headings... you can follow this [cheatsheet](https://www.markdownguide.org/cheat-sheet/).

If you want a **link** to be referenced in the *Links & Papers* section you just have to do ```[text](link:website_id)``` 
and add this "link" section in the yaml heading of the markdown (at least id & title & website)
```
links:
  - id: github-danclab # this is the website_id
    title: GitHub DANClab
    description: Where we have all our codes
    website: https://github.com/danclab
    image: https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png
```  
If you want just a standard link that is not really important you can just do this: ```[text](https://website.fr)``` 

If you reference a **paper**, you have to put it under data/papers/yourname.json, with at least a "citation-key", "title" and "URL" key
Why you have to cite the papers like this is that to simplify your task you can just go to your zotero, 
Select all the papers that you cite and then "export bibliography" as CSL/JSON and copy paste the exact output in data/papers/yourname.json. 

You can see the example in /data/papers/example.json
```
{
    "citation-key": "test",
    "title": "This is a test paper",
    "DOI": "https://url_of_the_test_paper.fr",
    "author": [                
        {
        "family": "Queen",
        "given": "John"
        }
    ],
    "issued": {
        "date-parts": [
            [
                2012,
                6
            ]
        ]
    }
}
```
Then you can cite the [PAPER](paper:test)

If you want a bibliography you can use the this code at the end of the document: ```{.{< bibliography>}}``` (just remove the dot)

Also, as this website is based on a template (which is only a bit modified), most template functionnalities can be used, 
see the documentation [website](link:caiwebsiteguide). 

{{<bibliography>}}