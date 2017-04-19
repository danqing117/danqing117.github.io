---
layout: post
title: How to Login in Python Web Scraping
description: Last year I have done a project to analyze Chinese students' experience in Halifax through searching some attractions' name on <a href="http://markdown.tw">Weibo</a> (a Chinese social media), and it requires me to scrape some user's data. The main obstacle is that the data is not visible until you have logged in. Therefore, this blog will talk about login technique in web scraping.
category: blog
---

Last year I have done a project to analyze Chinese students' experience in Halifax through searching some attractions' name on <a href="http://markdown.tw">Weibo</a> (a Chinese social media), and it required me to scrape some user's data. The main obstacle is that the data is not visible until you have logged in. Therefore, this blog will focus on login approach in web scraping. I will take <a href="http://markdown.tw">Weibo</a> as an example. The full code can be found on my <a href="https://github.com/danqing117/Weibo-Scraper-Python-">Github</a>.

## Requirements
The code is written in Python 3.0 including the following packages:

    import requests
	from bs4 import BeautifulSoup
	import re, json
	from urllib.parse import quote_plus
	import base64, rsa, binascii
	
Package <a href = "http://docs.python-requests.org/en/master/"> requests </a> is used for performing HTTP methods (some people may prefer <a href = "https://docs.python.org/2/library/urllib.html"> urllib</a> in Python 2), and it is the core package in web scraping. <a href = "https://www.crummy.com/software/BeautifulSoup/bs4/doc/"> BeautifulSoup</a>	is my personal love to extract data from HTML or XML files. Other packages here are used for regular regression, encoding, encryption, etc., and they are not necessary for all the other website. Studying the website is important before you decide which package you are going to use.

## Performing HTTP Methods

HTTP is a protocol to enable communications between clients and servers. Two commonly used methods are GET and POST. For example, when we open a new web page and browse the information shown on that page, we are sending a GET method to request data from a specfic url. When we input a username and a password to login a website, we are sending a POST method to submit data to the website you are browsing.  

To perform GET and POST method in Python code, the first step is to create a <a href="http://docs.python-requests.org/en/latest/user/advanced/"> Session object </a>. This object is to persist certain parameters across requests and it also stores cookie information.

	session = requests.Session()
	
Second step is to construct session headers. This is optional because Python will construct a default headers for us, however, if you are worrying about some webpages may block Python clients, you can construct headers by yourself to make your code look like a real Brower. To check requests headers on your own browser, you can right click to inspect the website and then click on Network to see the request header.

	agent = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/46.0.2486.0 Safari/537.36 Edge/13.10586"
	header = {
            "User-Agent": agent  
        }
	
To perform GET method, we simply write code as following:

	server_url = "https://login.sina.com.cn/signup/signin.php"
   	r = session.get(server_url, headers = header)              # response from server_url
	s = BeautifulSoup(r.content,"lxml")                        # extract lxml data
	print(s)

To perform a POST method to login, we need to construct post data (commonly they are only username and password) and then use post function.

	r = session.post(server_url, data = postdata, headers = header)
	
## Constructing Post Data
This part is the most difficult step because you must study the webpage very carefully and figure out what parameters you have sent to the server when you input your username and password. Even harder, some website can encrypt your username and password, like Weibo, therefore, it can take you time to dig into all the details.

To find out the post data when I login <a href="https://login.sina.com.cn/signup/signin.php">Weibo's login page</a>, first, I tried to login with a wrong a password (in case the website will relocate to a new page after I successfully logged in) and see the preserved log.

![Git Bash](/images/githubpages/weibo_login_postdata.png)

We construct the post data as a dictionary with keys shown in the picture above.

	postdata = {
            "entry":"sso",
            "gateway":"1",
            "from":"null",
            "savestate":"30",
            "useticket":"0",
            "pagerefer":"",
            "vsnf":"1",
            "su":su,
            "service":"sso",
            "servicetime": servertime,
            "nonce": nonce,
            "pwencode":"rsa2",
            "rsakv": rsakv,
            "sp":pwd_encry,
            "sr":"1280*720",
            "encoding":"UTF-8",
            "cdult":"3",
            "domain":"sina.com.cn",
            "prelt":"309",
            "returntype":"TEXT"
        }
		
Here, "su" is the username encoded with base64 encoder, and "sp" is the password encrypted with RSA code. Please check my code to see how to find those parameters.

## Relocation
Weibo's login page will relocate to a new page when you successfully logged in. To find out the relocation url, we need to do the regular regression on the response lxml data after we send a POST request. After that, we can send a GET request to the relocation url and scrape whatever we want.

## Some Results
In the end, I would like to show some results of my project. With running this <a href="https://github.com/danqing117/Weibo-Scraper-Python-">Weibo Scraper</a>, all the posts under a searching keywords are saved in an excel file with user's id and some other information if they are public.

![Git Bash](/images/githubpages/excel.png)

All the images under each post are saved in a file named by the post id.

![Git Bash](/images/githubpages/pics.png)









        



	

  






