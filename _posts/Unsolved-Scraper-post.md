
layout: post  
theme: jekyll-theme-dinky  
title: "Scraping Homicide Data"  
date: 2022-07-12 2:23:00 -0000  
categories: BeautifulSoup WebScraping DataCollection  
filename: Unsolved-Scraper-post.md  
permalink: /scraping-unsolved-homicide-data/  


#  Unsolved Homicides
For this project I've been analyzing a data set of all of the unsolved homicides in Cincinnati, OH from 1953 to 2021. I chose this data based on a few factors:   
		- 1. It was publicly available  
		- 2. I thought the subject matter was important  
		- 3. It had time and spatial aspects to it  
		- 4. I thought there would be some trends I could highlight  
When I first started this project I knew I wanted to create some basic charts to highlight any trends in the data such as common age of the deceased or time periods with more unsolved cases than others. First I had to get the data. I chose to get the data from the Ohio Attorney General's website. Unfortunately this website didn't have an easy way to download a * .csv file or an obvious API for me to use. So I wrote a webscraper.  
### Getting the data
 I had been meaning to learn how to write webscrapers for a while but this was the first time that I actually wrote one. Since I am using python I had a couple options: BeautifulSoup, Scrapy, and Selenium. I had more or less already decided on using BeautifulSoup for no reason other than I had skimmed the documentation a few times before.   
 
 After searching for a ``robots.txt`` file associated with the AG's page and not finding one. I started looking into the page source code. The Ohio AG's web page has records from all over the State so I narrowed it to only those in the Cincinnati Area; 48 pages of search results. Then I took pages that I felt were representative of what I was interested in and copied their source code into text files. I copied source code from a couple pages of search results as well as a few pages that held homicide case data. I used this data when developing my webscraper so that I didn't make too many requests to the AG's web page or inadvertently flood them with requests if I made a typo. 
 
 My plan for collecting the case data I needed was to first collect the URLs to each case on the pages of search results. Since the search results URL only changed the page number I was able to quickly create a function that would iterate over the results.
 
```python
# a generator object of links for 48 result pages of Cincinnati unsolved homicides
links = (("https://www.ohioattorneygeneral.gov/Law-Enforcement/Local-Law- 
		  Enforcement/Cold-Case/"
          f"Cold-Case-Search-Results?searchtext=Cincinnati&searchmode=anyword&page=
	          {numb}") for numb in range(1,49))

def traverse(link_gen, case_list):
    '''traverse search result pages and calls case_collector'''
    for link in link_gen:
        time.sleep(1) # 1s sleep between calls is probably the minimum sleep time
        page = BeautifulSoup(requests.get(link).content, 'html.parser')    
        case_collector(page, case_list)        		  
```

With that list I would then visit each URL 

```python
def case_collector(soup_obj, case_list):
    '''takes a BeautifulSoup object and passes list of urls of cold cases to scraper'''    
    headers = soup_obj.find_all(class_="mpHeader")
    for h in headers:
        case_list.append(h.find(class_='mpName', href=True)['href'])
    scraper(case_list)
```

and scrape the case data and store it in a hash table indexed on the case number. All of the data was then saved to a * .csv file for analysis in the next stage.

```python
def scraper(case_list):
    '''calls case_details table into hash table of cases. Returns complete hash table of cold cases'''
    for url in case_list:
        s = BeautifulSoup(requests.get(url).content, 'html.parser')
        scraped_cases[case_details(s)['Case number']] = case_details(s)
        time.sleep(1) # 1s between calls is probably the minimum sleep time
    case_df = pd.DataFrame.from_dict(scraped_cases)
    case_df.to_csv(path_or_buf='unsolved_OH.csv')
    return scraped_cases
```

So there it is!   
Those three functions (plus a 4th that extracts the data) are it. When I was writing the scraper it seemed much more involved but now that it's done it's a bit strange that four functions are all it takes to write a web scraper that iterates over 48 pages of search results, compiles a list of case URLs, then goes to each case, and turns this:  

```html
<div class="mpPhoto">
    <a href="/getattachment/f8693478-8b41-4923-881b-164f87f9ceb2/attachment.aspx" target="_blank"><img src="/getattachment/f8693478-8b41-4923-881b-164f87f9ceb2/teaser.aspx" class="mpImgDefault" alt="Rafeal Ross" /></a>
</div>
    
<ul>
                <li><strong>Case number: 618</strong></li>
                
                <li><strong>Incident location:</strong> 3540 Reading Road<br />Cincinnati, 
                Ohio
                 - Hamilton County
                <li><strong>Incident date:</strong> 1/2/2011</li>
                <li><strong>Homicide date:</strong> 1/2/2011</li>
                
                <li><strong>Gender:</strong> Male</li>
                <li><strong>Race/Ethnicity:</strong> Black</li>
                
                
                
                
                <li><strong>Law enforcement agency:</strong> Cincinnati Police Department</li>
</ul>

<h2>Details</h2><p> Twenty two year old Rafeal Ross was shot at 3540 Reading Road, Cincinnati, OH.</p>
```

into this:

```
	 618
1	 618
2	 3540 Reading RoadCincinnati, 
                Ohio
                 - Hamilton County
                Incident date
3	 1/2/2011
4	 1/2/2011
5	 Male
6	 Black
7	 Cincinnati Police Department
8	 Twenty two year old Rafeal Ross was shot at 3540 Reading Road, Cincinnati, OH.
```

After around 8-10 hours of slow paced work during the evenings I had the data I was after. I could start the analysis I had originally set out to do. Now I had to load it in to pandas and clean it up. As the webscraper saved all of the data in columns rather than rows. The first thing I'd need to do would be to transpose the data into a format that was a little better behaved.
