# Job-Recommendation-1.1-Web-scraping
We are going to work on a Job recommendation system, to match CV and Job posts. <br>
To create a databse for the job recommendations, we need to get data from the recruitment websites. <br>
We chose to web scrape some jobs posts from JobsDB (URL: https://hk.jobsdb.com/hk)
### 1. Understanding of websites
First of all, we have to understand how the websites work and how the URL link can access to certain page. 
Literally, how to go from main page to every job posts links
1. The website provide a direct way to access to the information we want. For example, if we select the job functions - information & technology, it reflects "https://hk.jobsdb.com/hk/jobs" + "information-technology"+"1"(page no.) 
![GitHub Logo](/images/logo.png)
![Job function](/Users/cleo/Documents/constance/Xccelerate/FTDS/FTDS Student Material/_Constance Project/5 Capstone/Photos for Github/Job function.png)
<img src="/Users/cleo/Documents/constance/Xccelerate/FTDS/FTDS Student Material/_Constance Project/5 Capstone/Photos for Github/Job function.png"/>
<img src=“/Users/cleo/Documents/constance/Xccelerate/FTDS/FTDS Student Material/_Constance Project/5 Capstone/Photos for Github/Job function.png” raw=true />
![Alt text](/Users/cleo/Documents/constance/Xccelerate/FTDS/FTDS Student Material/_Constance Project/5 Capstone/Photos for Github/Job function.png?raw=true "Title")
3. we want to know how many total pages in the job function, we found out there's a button showing no. of pages at the bottom of the page.
4. After we want to get all the jobs links from the page, e.g. (URL: https://hk.jobsdb.com/hk/en/job/network-and-system-administrator-100003008595010?jobId=jobsdb-hk-job-100003008595010&sectionRank=32&token=0~b1968878-db1a-40fd-a3ee-778ac96d973d&searchPath=%2Fhk%2Fjobs%2Finformation-technology%2F2)
5. in each job link / post, we want to get almost all information for our projects as database, namely, title, company name, job descriptions, requirements, benefits and so on.
### 2. The code 
1. import all the library we will use
2. we first set the function to generate all the links with job page, there's always around 250+ pages for the industry - information & technology
3. from each job page, we gather all the jobs posts links, there's roughly around 8,000 jobs, so here we get around 8,000 job links
4. from each job links, we will get all the information we need and put them into a dataframe
5. save the dataframe as CSV file
![Save as csv](/images/logo.png)
