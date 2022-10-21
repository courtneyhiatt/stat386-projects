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

Being a student at BYU in Provo, I'm interested to see if I could have gotten better quality professors at another BYU campus. Is there is a difference in professor 'quality' (based off of their overall rating on <a href="https://www.ratemyprofessors.com/">RateMyProfessors.com</a>) between the 3 BYU campuses (Provo, Idaho, and Hawaii)? I will perform an EDA on this data in a later post. For now, let's start with getting the data. 

### Webscraping or API?

In general, it will be much easier to gather data through an API, if available. For Rate My Professor, there isn't an official API

```
import pandas as pd

for i in stuff:
    cnt += 1
    if x < 10:
        try:
            something
        except:
            continue
    
return None
```
