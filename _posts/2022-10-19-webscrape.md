---
layout: post
title:  "Grading YOUR Professors: Scraping Ratings from Rate My Professors"
date:   2022-10-18
author: Courtney Hiatt
description: <a href="https://www.ratemyprofessors.com/">Rate My Professors</a> has been around since 1999, helping students choose their professors and courses for decades. Want to take a look at the data for your school's professors? Here's a guide on how to use Python to scrape info on your school.
image: /assets/images/rmp.jpeg
---
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script> 
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

The code in this blog post has been adapted from <a href="https://github.com/tisuela/ratemyprof-api">this github repository</a>. 

### Introduction
As a student, it has always been hard to choose professors. From general requirements to major courses, professors' teaching quality and styles can differ so much. <a href="https://www.ratemyprofessors.com/">Rate My Professors</a> gives students an opportunity to be open and honest about their experiences with a professor, and thus provides valuable information to other students looking to take the course and to the professors themselves (we can all use some constructive feedback at times). 

Being a student at BYU in Provo, I'm interested to see if I could have gotten better quality professors at another BYU campus. Is there is a difference in professor 'quality' (based off of their overall rating on Rate My Professors) between the 3 BYU campuses (Provo, Idaho, and Hawaii)? I will perform an EDA on this data in a later post. For now, let's start with getting the data. 

### Webscraping or API?

<p style="text-align:center;"><img src="https://github.com/courtneyhiatt/stat386-projects/raw/main/assets/images/scrapevsapi.png" alt="" style="width:800px;"/></p>

In general, it will be much easier to gather data through an API, if available. Unfortunately for us, Rate My Professor doesn't have an official API. But since this is probably some pretty popular data to scrape, a kind soul in the github world has <a href="https://github.com/tisuela/ratemyprof-api">posted their Python class</a> to scrape data specifically from Rate My Professors. My purposes differ a little bit than theirs, so I adapted the code to fit my needs and will walk through it below. 

### Step 0: Is this legal??

Although webscraping is completely legal, some websites ask that you don't scrape certain things (or at all). Looking at their <a href="https://www.ratemyprofessors.com/terms-of-use">Terms of Use</a>, Rate My Professor says: *"The Site is to be used solely for your noncommercial, non-exclusive, non-assignable, non-transferable and limited personal use and for no other purposes."* Because I'm not getting paid to analyze this data and my extent of use does not go beyond the scope of my data science class, I think it's okay to scrape. However, I did email them for written consent, but they haven't gotten back to me yet at the time of writing this post.


### Step 1: Getting Started in Python

The packages needed to use my code are: ```pandas```, ```json```, ```math```, ```os```, and, most importantly, ```requests```. The ```requests``` package allows ```get``` requests - the heart and soul of webscraping. 

You will also need to grab your school's ID from Rate My Professor. If you go to your school's page, you will find it at the end of the url. It should be anywhere between 1-4 digits. 

### Step 2: Get Requests

This webscraper works by cycling through pages of professors and making ```get``` requests on each, so we need a for loop. But first, we need to check how many pages of professors our school of interest has. I put the following code inside a function called ```get_num_of_professors(id)``` with the school ID as an argument.

```
page = requests.get(
  "http://www.ratemyprofessors.com/filter/professor/?&page=1&filter=teacherlastname_sort_s+asc&query=*%3A*&queryoption=TEACHER&queryBy=schoolId&sid="
  + str(id)
) 
temp_jsonpage = json.loads(page.content)
num_of_prof = (temp_jsonpage["remaining"] + 20)
```
This returns the number of professors at a school, but we can easily change this to pages of professors by dividing the number by 20 (there are 20 professors listed per page). 

Next, I make a ```get``` request on each page of professors to scrape their rating and identifying information, then append it to a ```pandas``` dataframe. The code is for my ```scrape_profs(id: str)``` function is below.

```
professors = dict()
num_of_prof = get_num_of_professors(id)
num_of_pages = math.ceil(num_of_prof / 20)
df = pd.DataFrame()

for i in range(1, num_of_pages + 1):
  page = requests.get(
    "http://www.ratemyprofessors.com/filter/professor/?&page="
     + str(i)
     + "&filter=teacherlastname_sort_s+asc&query=*%3A*&queryoption=TEACHER&queryBy=schoolId&sid="
     + str(id)
   )
   json_response = json.loads(page.content)
   temp_list = json_response["professors"]

   for json_professor in json_response["professors"]:
      df1 = pd.DataFrame(json_professor, index=[0])
      df = pd.concat([df,df1], ignore_index=True)
```

This gives a complete dataframe of the school's professor information, which includes a lot of variables that aren't really of interest to me. I decided to keep the school name and ID for easy comparison between the BYU campuses, the professor's first, last, and middle name, the number of ratings they have, and their overall rating (out of 5). Scraping the 3 campuses, I ended up with 7947 professors, more than half of which came from the BYU Provo campus. My full code and scraped dataset can be found in <a href="https://github.com/courtneyhiatt/rmpBYU">this github repository</a>. 

### Conclusion

In this post, I went over my process for scraping data from Rate My Professors. In a later post, I will perform an exploratory data analysis on my data to begin testing for any relationships between schools and overall professor ratings. If you liked my code or have any suggestions on making it better, comment below! 


