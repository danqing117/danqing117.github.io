---
layout: post
title: How to Login in Web Scraping (Weibo Example in Python 3.0)
description: Last year I have done a project to analyze Chinese students' experience in Halifax through searching some attractions' name on <a href="http://markdown.tw">Weibo</a> (a Chinese social media), and it requires me to scrape some user's data. The main obstacle is that the data is not visible until you have logged in. Therefore, this blog will talk about login technique in web scraping.
category: blog
---

Last year I have done a project to analyze Chinese students' experience in Halifax through searching some attractions' name on <a href="http://markdown.tw">Weibo</a> (a Chinese social media), and it required me to scrape some user's data. The main obstacle is that the data is not visible until you have logged in. Therefore, this blog will talk about login technique in web scraping.

<a href="https://github.com/danqing117/Weibo-Scraper-Python-">Code link</a> 

## Requirements
The code has included the following packages:

	import requests
	from bs4 import BeautifulSoup
	import re, json
	from urllib.parse import quote_plus
	import base64, rsa, binascii
	
R	
	

  






