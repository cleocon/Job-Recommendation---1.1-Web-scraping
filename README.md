# Job-Recommendation-1.1-Web-scraping
We are going to work on a Job recommendation system, to match CV and Job posts. <br>
To create a databse for the job recommendations, we need to get data from the recruitment websites. <br>
We chose to web scrape some jobs posts from JobsDB (URL: https://hk.jobsdb.com/hk)

## 1. Understanding of websites

First of all, we have to understand how the websites work and how the URL link can access to certain page. 
Literally, how to go from main page to every job posts links
1. The website provide a direct way to access to the information we want. For example, if we select the job functions - information & technology, it reflects "https://hk.jobsdb.com/hk/jobs" + "information-technology"+"1"(page no.) 

![jobfunction](images/Job%20function.png)

2. we want to know how many total pages in the job function, we found out there's a button showing no. of pages at the bottom of the page.

![no.ofpage](images/no.%20of%20pages.png)

3. After we want to get all the jobs links from the page, e.g. (URL: https://hk.jobsdb.com/hk/en/job/network-and-system-administrator-100003008595010?jobId=jobsdb-hk-job-100003008595010&sectionRank=32&token=0~b1968878-db1a-40fd-a3ee-778ac96d973d&searchPath=%2Fhk%2Fjobs%2Finformation-technology%2F2)

![sample job links](images/sample%20job%20links.png)

4. in each job link / post, we want to get all the information for our projects as database, namely, title, company name, job descriptions, requirements, benefits and so on.

![all job posts details](images/all%20job%20details.png)


## 2. The code 

1. import all the library we will use
```
from bs4 import BeautifulSoup
import requests
from time import sleep
from random import randint
import re
import pandas as pd
from datetime import datetime
```
2. we first set the function to generate all the links with job page, there's always around 250+ pages for the industry - information & technology
```
# get all URL link
def URL_():
    URL_f = "https://hk.jobsdb.com/hk/jobs/information-technology/"
    html_f = requests.get(URL_f)
    function = BeautifulSoup(html_f.text, "html.parser")
    
    page = [i.text for i in function.select("option")]
    page_num = []
    
    for i in page:
        for j in i.split():
            if(j.isdigit()):
                page_num.append(j)
    URL_all = []
    for i in range(1,int(page_num[-1])+1):
        #URL_all = ("https://en.tripadvisor.com.hk/Restaurant_Review-g294217-d13283405-Reviews-or")
        #URL_b = ("-Yardbird-Hong_Kong.html")
        URL_combine = URL_f + str(i)
        URL_all.append(URL_combine)
    return URL_all

#activate function
URL_()
```
3. from each job page, we gather all the jobs posts links, there's roughly around 8,000 jobs, so here we get around 8,000 job links
```
#just save links
links = []
for i in URL_():
    html_e = requests.get(i)
    everyjobpage = BeautifulSoup(html_e.text, "html.parser")
    links_all =[a['href'] for a in everyjobpage.select('a[href]')]
    r = re.compile(r'.*token=')
    joblinks = list(filter(r.match, links_all))
    links.append(joblinks)

job_page = []
for i in links:
    for j in i:
        basic_URL = "https://hk.jobsdb.com"
        URL_combine = basic_URL+j
        job_page.append(URL_combine)

job_page
```
4. from each job links, we will get all the information we need and put them into a dataframe
```
#just save job details
startTime = datetime.now()

job_title = []
co_name = []
heading_info = []
job_highlights = []
job_d = []
add_info = []
co_overview = []
jobs = []

for i in job_page:
    html_d = requests.get(i)
    job = BeautifulSoup(html_d.text, "html.parser")
    #job title
    jt = job.h1.string
    job_title.append(jt)
    #co_name
    cn = job.find("div", {"data-automation":"detailsTitle"}).span.get_text()
    co_name.append(cn)
    #heading_info
    hi = [i.text for i in job.find_all(class_="FYwKg _11hx2_0")]
    heading_info.append(hi)
    #job_highlights
    jh = [i.text for i in job.find_all(class_="FYwKg _3VCZm _1uk_1 _3RqUb_0")]
    job_highlights.append(jh)
    #job_descriptions
    jd = job.find("div", {"data-automation":"jobDescription"}).get_text()
    job_d.append(jd)
    #Additional info
    ai = [i.text for i in job.find_all(class_="FYwKg _1uk_1 _2fqoM_0 _1hqiH_0")]
    add_info.append(ai)
    #company overview
    co = [i.text for i in job.find_all(class_="vDEj0_0")]
    co_overview.append(co)
    jobs_all=pd.DataFrame({
    "title":job_title,
    "company":co_name, 
    "heading info":heading_info,
    "job highlights":job_highlights,
    "job description":job_d,
    "additional information":add_info,
    "company overview":co_overview})
    #jobs.append(jobs_all)


print(datetime.now() - startTime)
```
5. save the dataframe as CSV file
```
jobs_all.to_csv("sciences-lab-research-development.csv")
```
