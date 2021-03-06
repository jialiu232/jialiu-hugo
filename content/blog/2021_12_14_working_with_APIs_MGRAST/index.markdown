---
title: "Working with APIs in Python"
author: "Jia Liu"
date: '2021-12-14'
excerpt: Using the `requests` python library to make an API call and collect data
subtitle: ''
draft: no
images: null
series: null
tags: null
categories:
  - python
  - API
layout: single
#output: html_document
---






## So why are we here?

Sometimes you may want to retrieve data from some publicly available data sources, such as youtube, netflix, or twitter, for your data science projects. Then you will probably need to work with APIs created by these data sources.   

In this post, we will write a python command line script to retrieve some functional gene information from [MG-RAST](https://www.mg-rast.org/), *an open source, open submission web application server that suggests automatic phylogenetic and functional analysis of metagenomes*.

---

## Goal

MG-RAST has a functional gene database, where each functional gene has a unique id, m5nr id, and each m5nr id has a list of information such as: what is the corresponding KO id (functional gene id from another popular gene database) of this m5nr id; what functions does this m5nr id has. I have a file `m5nr_ids_3rd_00` that each line is a unique m5nr id as shown in img.1 below. For each line/m5nr id, I want to retrieve the corresponding KO id as well as functional information from the MG-RAST database. Finally, I want to make a loopup document that looks like img. 2.  


<center>

![](m5nr_file.jpg)

img.1  m5nr ids

</center>



<center>

![](m5nr_info.jpg)

img.2  m5nr id lookup document

</center>



### Workflow

Let's create a new python file `get_KO_ids_from_m5nr.py` and start working in it!

#### Prepare

First we will import the libraries needed. `requests` will be used to request information from the database for each id; I plan to make this script a command line python tool, so we will use `argparse` to parse the arguments for the command; we will explain more about `time` later.


```python
import requests
import argparse
import time
```


This command line script takes the file with unique m5nr ids as its lines as its input. It outputs a file within which each line will have a m5nr id, its corresponding KO id, and the function information being separated by tab. Let's make a parser for the command line arguments and define the input file and output file variables: 


```python
parser = argparse.ArgumentParser(description="Fetch the KO id for each m5nr id (for each line)")
parser.add_argument("inputFile", type=str)
args = parser.parse_args()

inputF = args.inputFile
outputF = inputF + ".book"
```


#### Retrieve information using API

- A small test on MG-RAST API for one m5nr id

Let's take one m5nr id, `021412f0cd017376a6024e9d30f655f2`, as an example. From the instructions of [MG-RAST API](https://help.mg-rast.org/api.html#example-scripts-using-the-mg-rast-rest-api), I found that to retrieve the annotation information I need, I should use a prefix `https://api.mg-rast.org/1/m5nr/md5/` before the m5nr id, and a suffix `?group_level=function&source=KO` after the id. So an URL for this id will look like [`https://api.mg-rast.org/1/m5nr/md5/021412f0cd017376a6024e9d30f655f2?group_level=function&source=KO`](https://api.mg-rast.org/1/m5nr/md5/021412f0cd017376a6024e9d30f655f2?group_level=function&source=KO). Click the link or browse the URL you will find the annotation information of the m5nr id `021412f0cd017376a6024e9d30f655f2` in `json` format. From that information, we can extract the KO id ("K01945") and function description ("phosphoribosylamine--glycine ligase [EC:6.3.4.13]").



- The real game

Retrieving the information with a single URL as described above seems easy, while in reality you may need to make hundreds and thousands of retrieves. In my project, I need to request the information from MG-RAST for `\(835902\)` unique m5nr ids. Let's start the real game and see how we can do it in python.  

For each line/m5nr id of the input file, we will (1) build an URL for API calling, (2) retrieve the full KEGG annotated (KO) information from MG-RAST and (3) convert to json format, (4) finally extract the KO and function information and write to the output file.


```python
fout = open(outputF, 'w')

with open(inputF, 'r') as f:
	for line in f:
		line = line.strip()
		
		# 1. Build the URL that will be used for API call the current m5nr id 
		newLine = "https://api.mg-rast.org/1/m5nr/md5/" + line.strip() + "?group_level=function&source=KO"
		print(newLine)

		# 2. Make the API call and save the the response to variable r 
		r = requests.get(newLine, timeout=300)
		
		time.sleep(2)
		
		# 3. Get the response in json format if response contains a valid JSON.
		res = ""
		if r.status_code != 204:
			res = r.json()
		else:
			print("Error 204")
			continue

    # 4. Extract KO id and function information from the json output and write the result into output file
		ko = res['data'][0]['accession']  
		# print(ko)

		func = res['data'][0]['function']
		# print(func)

		fout.write(f'{line}\t{ko}\t{func}\n')

fout.close()
```


Note that when many requests are made against a database in a short time, the database may not be able to give normal responses. `time.sleep(2)` is used in the script to suspends execution for 2 seconds. It helped a little bit in my case, but for all the `\(835902\)` m5nr ids I need to process, there is still a long way to go. 


## Conclusion

There is not many resources or tutorials about how to retrieve m5nr id information from MG-RAST. The [MG-RAST API](https://help.mg-rast.org/api.html#example-scripts-using-the-mg-rast-rest-api) is well organized and maintained, but can be too broad for specific requests. This blog describes how I am working with MG-RAST APIs to retrieve the data I need. I hope it could be helpful to someone, and please let me know if there is any better ways to interact with MG-RAST API or APIs in general! The next step, I plan to work with the APIs of some other publicly available data sources (netflix maybe?) to collect data for my project. 
