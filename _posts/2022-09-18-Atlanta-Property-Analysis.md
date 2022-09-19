---
title: Atlanta Property Analysis   
date: 2022-09-18  
categories: [Data Science,  GIS, Pandas, QGIS, Socioeconomics]  
permalink: /_posts/2022-09-18-Atlanta-Property-Analysis.md  
---
# Atlanta Data Analysis
## Starting out
I've been meaning to brush up on my GIS skills for a while and I've finally gotten around to it. I'll be using pandas and QGIS (version 3.18.2) for this series of posts. I'm aware that there's more up to date versions out there but I already had this installed. My plan for this series of posts is to look at how the city of Atlanta has changed over the past decade. For now I'll be focusing on just the city proper and not the larger metropolitan area. 
I was able to find the relevant data for this exercise and analysis through a number of local government websites. The Fulton County Tax Commissioner, the Georgia Geospatial Information Office, and the Georgia Association of Regional Commissions host most of the data I will be using here.   

## Mapping 
Since I'm a bit rusty with QGIS I thought I'd start out pretty basic. Using the 2010 demographic census tract data I decided to look at where people were living. As of [the 2010 census](https://www.atlantaga.gov/home/showpublisheddocument/4383/636245709418400000) African Americans(54%) and Caucasians (38%) account for 92% of the population so the map below isn't able to show much detail for demographics outside of those groups.   
![A map of the Demographics of Atlanta](/docs/assets/AtlantaNeighborhoodDemographics.svg)
<figcaption align="center">
	(<i>Figure 1</i>)   
	</figcaption>  
	
As you can see in *figure 1*,  Atlanta was a fairly segregated city in 2010. As of [2019](https://belonging.berkeley.edu/most-least-segregated-cities) it is ranked at #11 by Berkley researchers at the Othering & Belonging Institute. Only 5 neighborhoods in the city had less than 50% of both African Americans and white Americans in 2010. 

### Closing
This was a short post but it was more about re-familiarizing myself with the menus in QGIS and exploring the data a little bit. I've started analyzing some tabular data in pandas and I'll discuss some of the questions I want to explore in upcoming posts.  I'm planning on looking at the demographics of the city, property value, property sales, and changes in the types of buildings present.

## Data sources
#### State wide
- [Population Change](https://opendata.atlantaregional.com/datasets/GARC::population-change-1990-2020-by-zip-code-1/explore?location=33.255502%2C-84.052413%2C9.58&showTable=true)  
#### Fulton County   
- https://www.fultoncountyga.gov/maps    
- http://share.myfultoncountyga.us/datashare/fultoncounty/data/Demog_CensusTracts2010Profile/SHP/Demog_CensusTracts2010Profile.zip    
- [Georgia Geospatial Information Office](https://gio.ga.gov/)  
- [Georgia Association of Regional Commissions](https://opendata.atlantaregional.com/datasets/GARC::major-roads/explore?location=33.706256%2C-84.434300%2C9.53)  
