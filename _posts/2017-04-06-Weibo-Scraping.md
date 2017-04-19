---
layout: post
title: How to Login in Python Web Scraping
description: Last year I have done a project to analyze Chinese students' experience in Halifax through searching some attractions' name on <a href="http://markdown.tw">Weibo</a> (a Chinese social media), and it requires me to scrape some user's data. The main obstacle is that the data is not visible until you have logged in. Therefore, this blog will talk about login technique in web scraping.
category: blog
---

Last year I have done a project to analyze Chinese students' experience in Halifax through searching some attractions' name on <a href="http://markdown.tw">Weibo</a> (a Chinese social media), and it required me to scrape some user's data. The main obstacle is that the data is not visible until you have logged in. Therefore, this blog will talk about login technique in web scraping.

<a href="https://github.com/danqing117/Weibo-Scraper-Python-">Code link</a> 

## Requirements
The code is written in Python 3.0 including the following packages:

	import requests
	from bs4 import BeautifulSoup
	import re, json
	from urllib.parse import quote_plus
	import base64, rsa, binascii
	
Package <a href = "http://docs.python-requests.org/en/master/"> requests </a> is used for performing HTTP methods (some people may prefer <a href = "https://docs.python.org/2/library/urllib.html"> urllib</a> in Python 2), and it is the core package in web scraping. <a href = "https://www.crummy.com/software/BeautifulSoup/bs4/doc/"> BeautifulSoup</a>	is my personal love to extract data from HTML or XML files. Other packages here are used for regular regression, encoding, encryption 
	

  






