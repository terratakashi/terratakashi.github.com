---
layout: post
title: "Fetch Data From Broken Page"
date: 2013-07-20 01:13
comments: true
categories: [Ruby, Regexp] 
---

This was happening to me yesterday.  I tried to fetch some strings from ancient static pages which were made by frontpage before 10 years ago. 

The old page may look like this:
{% codeblock lang:html %}	
	<html>

	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=big5">
	<meta name="GENERATOR" content="Microsoft FrontPage 5.0">
	<meta name="ProgId" content="FrontPage.Editor.Document">
	<title>比利時粗毛獵犬</title>
	</head>

	<body bgcolor="#FFCC99">

	<p align="center"><img border="0" src="74-1.jpg" width="327" height="432"></p>
	<p align="center"><font color="#0000FF" face="正謙重圓黑框字形" size="7">比利時粗毛獵犬</font>

	<p align="center"><b>
	<span lang="EN-US" style="font-size: 16pt; color: teal; font-family: 標楷體; background: yellow">
	<a style="color: blue; text-decoration: underline" href="http://www.abcd.com/NEW_PAGE_14.HTM">
	犬種圖鑑百科</a></span><span lang="EN-US" style="FONT-SIZE: 16pt; COLOR: teal; FONT-FAMILY: 標楷體">&nbsp;
	</span>
	<span lang="EN-US" style="font-size: 16pt; color: blue; font-family: 標楷體; background: yellow">
	<a style="color: blue; text-decoration: underline" href="http://www.abcd.com/NEW_PAGE_13.HTM">
	貓種圖鑑百科</a></span><span lang="EN-US" style="FONT-SIZE: 16pt; COLOR: blue; FONT-FAMILY: 標楷體">&nbsp;
	<span style="background: yellow">
	<a style="color: blue; text-decoration: underline" href="http://www.abcd.com/new_page_204.htm">
	寵物兔圖鑑百科</a></span> <span style="background: yellow">
	<a style="color: blue; text-decoration: underline" href="http://www.abcd.com/new_page_193.htm">
	錦鯉品種圖鑑百科</a></span> <span style="background: yellow">
	<a style="color: blue; text-decoration: underline" href="http://www.abcd.com/">
	回首頁</a></span></span></b><p align="left">原產地：比利時<p align="left">身高：20公分
	<p align="left">體重：3.5公斤至4.5公斤。5.4公斤以下。<p align="left">歷史：本犬種是從17世紀時，由德國的篤賓犬和名利時的當地大改良而來，到了19世紀才成為現在的樣子。最初改良的目的是為捕鼠。<p align="left"><br>
	特徵：可斷尾或斷耳，聰明乖巧<br>
	，十分渾圓。<p align="left">毛色毛質：分為粗毛與順毛二種，粗毛種的毛質粗，順毛則短有彈性，順毛種無黑色，主要顏色有暗褐、紅褐色，夾鬚與顎頭鬚的顏色較深。<p align="left"><br>
	附註：本犬種的數量不多，    但卻有許多狗迷</body></html>
{% endcodeblock %}
At first, I try to use `mechanize` to retrieve the particular text from dedicated area.  Howeve, due to the missing and messy tags, it didn't work by using simple selectors.  Therefore, I brutally fetch all text from the page.  

Also, I found out there are several unsual whitespace characters within it, so I got rid of all white space characters which include line changing, tab, and space. 

The code looks like this:
{% codeblock lang:ruby %}
	content = page.parser.text.gsub(/\s/, "")
{% endcodeblock %}
Finally, I got a raw text data by repeating tests, and I need to deal with the sticky text.  Fortunately, the page was talking about the classification.  So I can easily to punctuate the content by using regular expressions, and add line changing before every categories.

This is the code for punctuating the text:
{% codeblock lang:ruby %}
    kind_list = ["原產地", "身高", "體重", "歷史", "特徵", "個性", "毛色毛質", "附註"] 
    kind_list.each do |kind|
      content = content.sub(kind, "\n#{kind}")
    end
{% endcodeblock %}
And the result is here:
	名稱-比利時粗毛獵犬
	原產地-比利時
	身高-20公分
	體重-3.5公斤至4.5公斤。5.4公斤以下。
	歷史-本犬種是從17世紀時，由德國的篤賓犬和名利時的當地大改良而來，到了19世紀才成為現在的樣子。最初改良的目的是為捕鼠。
	特徵-可斷尾或斷耳，聰明乖巧，十分渾圓。
	個性-
	毛色毛質-分為粗毛與順毛二種，粗毛種的毛質粗，順毛則短有彈性，順毛種無黑色，主要顏色有暗褐、紅褐色，夾鬚與顎頭鬚的顏色較深。
	附註-本犬種的數量不多，但卻有許多狗迷

Regexp is so powerful.  I start to be addicted to it!