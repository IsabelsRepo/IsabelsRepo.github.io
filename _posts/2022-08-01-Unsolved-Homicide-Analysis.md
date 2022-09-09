---
title: Unsolved Homicide Data Analysis     
date: 2022-08-01  
permalink: /unsolved-homicide-analysis/   
---

# Unsolved Homicide Data Analysis
This is the probably the last post in a series of posts about my [collection](https://isabelsrepo.github.io/scraping-unsolved-homicide-data/), [cleaning](https://isabelsrepo.github.io/unsolved-homicide-data-cleaning/), and analysis of Unsolved Homicide data for the Cincinnati area. In the last post, I left off after briefly describing some of the cleaning I preformed on the data and described some of the data I added. Using this mostly clean data I created a few charts to demonstrate some of the trends in the data.  

The age distribution of the deceased had some surprises. I had heard that young people are more likely to be murdered than people in older age brackets but it was still interesting to see in this data. It was also very strange to me that so many very young people were likely to be murdered. Boys 11-20 were essentially as likely to be murdered as men 31-40. And then there were the extremely young. The number of children under 10 and more infants than I care to remember. It is absolutely heart breaking.   

![Age distribution of Homicides, categorized by gender](https://raw.githubusercontent.com/IsabelsRepo/IsabelsRepo.github.io/main/img/AgeDistribution.png)  
(*Figure 1*)   
I'm not sure what needs to be done to prevent such young people from being murdered but clearly we aren't doing it.  

So we've looked a little bit at  the breakdown of homicide cases by victim age but what about some other trends that might be a bit more odd. The dataset contains the date of each homicide (where a range was presented the initial date was presumed to be the homicide date). What's the deadliest month in Cincinnati?  

![Homicides by month. March is the deadliest](https://raw.githubusercontent.com/IsabelsRepo/IsabelsRepo.github.io/main/img/CasesByMonth.png)  
(*Figure 2*)  
It turns out that March is the deadliest month. Is this a spurious correlation or is there something to this? I'm not entirely sure. I could see it being either. March in Cincinnati is a bit unpredictable, it could be 75 and sunny or it could be 15 and snowing. Does the likelyhood or precipitation affect evidence that can be collected? Is it because there are fewer people out to be witnesses? Or is it because there's the weather is 'just right'? It's warm enough to be outside but cold enough to only be out for a purpose. There are other trends with [heatwaves](https://www2.psych.ubc.ca/~schaller/308Readings/Anderson2001.pdf) are known to increase violent crime and social aggression. Does March represent the same thing but in reverse? This might be interesting to try to correlate in other cities throughout the midwest and places that get 4 seasons. 

Since this is time series data, I was curious what it all looked like.  
![Unsolved Homicide Cases in Cincinnati. 1950-2021](https://raw.githubusercontent.com/IsabelsRepo/IsabelsRepo.github.io/main/img/CasesByYear.png)  
(*Figure 3*)  

What we see is that most of the unsolved cases have occured in the past 20 years or so. I guess I was going in expecting that there would be more unsolved cases the further back in history we looked. I based this on a general perception that forensics and investigative techniques have changed and improved as the decades passed. This doesn't seem to be the case though. If we look at annual homicides over roughly the same period we see the same general shape. Which is to be expected but it's still reassuring to see the patterns present across related data. 
![Cincinnati Enquirer Annual Homicide Totals In Cincinnati](https://www.gannett-cdn.com/presto/2021/12/27/PCIN/5c7b1ac9-f042-4a00-815c-3603eb002c10-copy-homicides-2020-cam_4.jpg?width=660&height=460&fit=crop&format=pjpg&auto=webp)   
(*Figure 4*)  
This chart prepared by the [Cincinnati Enquirer](https://www.cincinnati.com/story/news/crime/2021/12/27/cincinnati-approaches-another-record-year-homicides/9024445002/) shows that part of the reason behind more unsolved cases in the last 20 years than in the 50's-90's is due to an increase in homicides. Particularly we see a jump as we enter the 2000's. Why this happens is beyond the aims of this analysis but it's important to note the general trend. It's also important to note, *figure 1*, that from 2001-2007 the Cincinnati Police Department was taking part in a [Memorandum of Agreement](https://www.cincinnati-oh.gov/police/department-references/department-of-justice-agreement/) with the US Department of Justice. This agreement was entered following the riots sparked by the killings of [Roger Owensby, Jr.](https://en.wikipedia.org/wiki/Roger_Owensby,_Jr.) from asphyxiation, the shooting of unarmed [19-year-old Timothy Thomas](https://en.wikipedia.org/wiki/Cincinnati_riots_of_2001), and years of discriminatory policing by the department. Over this whole 20 year time period from 2001 - 2021 we see significant economic strain along with social strain from riots, terrorism, war, and now a global pandemic.  While I don't intend to try to explain the increase of homicides I feel like it's worth reminding people of these factors.  

After looking at the correlation between *fig 3* and *fig 4* and noting some of the local history, does race impact the cases?   
![Pie chart of homicides by race 74.4% of all unsolved cases are African American](https://raw.githubusercontent.com/IsabelsRepo/IsabelsRepo.github.io/main/img/CasesByRace.png)  
<figcaption align="center">
	(<i>Figure 5</i>)   
	</figcaption>


I haven't been able to look at all of the homicide data yet so I don't know if this mirrors the number of total homicides. I can say that according to [the census](https://www.census.gov/quickfacts/cincinnaticityohio) the demographics of Cincinnati are as follows: 
  
		White alone 50.3%
		Black or African American alone 41.4%
		American Indian and Alaska Native alone  0.1%, 
		Asian alone, percent 2.2%
		Native Hawaiian and Other Pacific Islander alone 0.1%
		Two or More Races 4.6%
		Hispanic or Latino 4.2%

Based on this data, African Americans are over represented in unsolved homicide cases.  

## Mapping the data
When I was first starting out with this project I knew I wanted to show the spatial relationship of these homicides. I used the folium library which is based on leaf.js (so make sure your script blockers let it through). The interactive map below should let you zoom and pan across the city. If you click on a marker you should be presented with the case number and some of the details of the case. The color of each case is a reflection of it's proximity to the present. The further back in time the case is the fainter the color, the more intense the color the more recently the person was murdered. Lastly, if you happen to have information relevant to one of the cases please contact the [Ohio Bureau of Criminal Investigation](https://www.ohioattorneygeneral.gov/Individuals-and-Families/Victims/Submit-a-Tip/Unsolved-Homicide-Tip.aspx)  


**Content Notice**: The information contained in these markers are homicide case notes, some of the material may not be suitable for all readers and may contain disturbing descriptions of violence.  
![Unsolved Homicide Map Test](img/Unsolved-Homicides-by-Gender.html)
![Unsolved Homicide Map][(https://isabelsrepo.github.io/Unsolved-Homicides-by-Gender.html](https://raw.githubusercontent.com/IsabelsRepo/IsabelsRepo.github.io/main/img/Unsolved-Homicides-by-Gender.html))  
![Unsolved Homicide Map 3](Unsolved-Homicides-by-Gender.html) 
![Unsolved Homicide Map Test 2](https://IsabelsRepo.github.io/blob/main/img/Unsolved-Homicides-by-Gender.html)

## Leaflet.JS
Leaflet.JS would not exist were it not for the hard work of [Volodymyr Agafonkin](https://agafonkin.com/) . Volodymyr is a Ukrainian citizen who has been displaced by the war and devastation brought on by the Russian invasion of Ukraine. Please consider contributing to one of the organizations helping those affected by the war. [Stand With Ukraine](https://stand-with-ukraine.pp.ua/) and [Come Back Alive](https://www.comebackalive.in.ua/) have been suggested by the [Leaflet.JS](https://leafletjs.com/2022/04/18/leaflet-1.8.0.html) team. 
