---
title: Isabel's Project Home Page
filename: index.md
---

# Welcome!  
Hello and [Welcome!](https://isabelsrepo.github.io/updates/blognews/2022/08/01/Welcome-Post.html)
I'm glad you found my projects. I am a biogeochemist and I like to use python to help me investigate data. I have a few projects I've been working on that you might want to take a look at.  

## Analyzing Atlanta Property records  
### [Mapping Demographics](https://isabelsrepo.github.io/Atlanta-Property-Analysis/)
When a census is taken the data is compiled into tables that show a breakdown for a variety of subregions. I chose to create a chloropleth map that depicts the dominant racial break down of each neighborhood within the City of Atlanta. While legal segregation is theoretically relegated to the history books social segregation is alive and well. My intention is to depict these trends in an easy to digest manner. Throughout the rest of this project I intent to look into the property value across both neighborhood subregions and individual parcels when it is practial. As I dive deeper into the the census data and parcel records for the past decade I hope to communicate some thought provoking questions.  

### [Atlanta Property Records Data Cleaning](https://isabelsrepo.github.io/Atlanta-Property-Record-Cleaning/)     
I left off the last post in this series talking about creating maps in QGIS. I preformed some basic analysis based on the demographic information that was available via the Fulton County government website. This post is going to pick up where that one left off. I won't be showing any maps here but I will be cleaning up 12 years of property records data and doing a little exploring with you.  

-------   

## Unsolved Homicides
### [Acquiring the data](https://isabelsrepo.github.io/scraping-unsolved-homicide-data/)
I collected some publicly available data on unsolved homicides in the Cincinnati area and I'm the process of analyzing that data. I have a brief post on how I collected the data and I'm currently writing a couple posts on some trends I noticed while looking at the data. 

### [Cleaning the data](https://isabelsrepo.github.io/unsolved-homicide-data-cleaning/)
In my last post I discussed scraping data about unsolved homicides. Here, I discuss some of the challenges I encountered cleaning that data and briefly describe my overall thought process and approach. By the end I have data that I'll use in a follow up post to create a map and some charts that help highlight some trends in the data.

### [Analysis](https://isabelsrepo.github.io/unsolved-homicide-analysis/)
This  is the probably the last post in a series of posts about my [collection](https://isabelsrepo.github.io/scraping-unsolved-homicide-data/), [cleaning](https://isabelsrepo.github.io/unsolved-homicide-data-cleaning/), and analysis of Unsolved Homicide data for the Cincinnati area. In the last post, I left off after briefly describing some of the cleaning I preformed on the data and described some of the data I added. Using this mostly clean data I created a few charts to demonstrate some of the trends in the data. 

**Content Notice**: The information revealed by clicking these markers is homicide case notes, some of the material may not be suitable for all readers and may contain disturbing descriptions of violence.

<iframe src="Unsolved-Homicides-by-Gender.html" height="600" width="600"></iframe>

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
