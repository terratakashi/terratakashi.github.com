---
layout: post
title: "Regexp: How to deal with Chinese words"
date: 2013-07-04 00:32
comments: true
categories: [Regexp, Ruby]
---

Today I'm trying to use regexp to catch some special words.  For Chinses documents, it need to use *utf8* to present the words, so `/w` is useless in this case.  

For this problem, we can have two choices to  slove it:

The first option, Chinese words distribute over `u4e00-u9fa5`, so we can use `[u4e00-u9fa5]` to catch them.  
The other option is using '[^x00-xff]' to detect the word which doesn't belongs to *ASCII codes*.
