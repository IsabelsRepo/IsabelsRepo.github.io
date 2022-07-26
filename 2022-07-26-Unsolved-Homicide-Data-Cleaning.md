# Cleaning Data  
  In the [first post](https://github.com/IsabelsRepo/IsabelsRepo.github.io/blob/main/Unsolved-Scraper-post.md) for this project I walked through writing my first webscraper using BeautifulSoup4 and I ended up collecting data on unsolved homicides in Cincinnati into a  * .csv file that looked like this:  
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

One thing you might notice is missing is a column for the age of the person when they were murdered. I ended up opening the data in excel and transcribing the age from the ```['details']```  entries.   
Working with data like this in columns is possible but it's a bit clunky. Most people prefer to work with data in rows since it allows you faster access to whole categories like ```['Incident location']```  rather than having to select each case and then select the category you're after.  I'm going to use [Pandas](https://pandas.pydata.org/) to clean and transform this data set. I like using this library and it's also very popular so a lot of common tasks have built in functions.   
The first step is pretty easy, load the data into a data frame and transpose it.  

![homicide case data transposed](https://github.com/IsabelsRepo/IsabelsRepo.github.io/blob/main/img/cases_df.T.png?raw=true)

So now I have the data organized in a more familiar format. The next stage is to start cleaning it up. I need to set the columns names to actually be the column names. When I loaded and transposed the data pandas used the indices as the column names.  Changing the columns is pretty straight forward, I used `case_df.columns`  and then set it equal to the entire row I wanted to use as column names. This leaves me with duplicate data but that's easily fixed by dropping the top row.   
From here, I decided to tackle the time. Currently the `['Incident date']` column is treated as a pandas object, this makes some of the things I want to do impossible. To fix that I'm going to change it into a date-time object which will let me do things like graph by year  and manipulate the data by year. While I was converting the data I got an error that some of my data wasn't in the proper format. So I made a copy of the data frame and set `errors='coerce'` . By using this option, any values that have a format that `pandas.to_datetime()` can't interpret get set to Not a Time;  `NaT`.  Now I could isolate the cases that had issues with their dates and manually fix it.   

```python
# Replace ranges of ['Incident date']s with initial date
date_error_df =  pd.to_datetime(case_df['Incident date'], errors='coerce')
nat_df = date_error_df[date_error_df.isna()==True]
nat_cases = list(nat_df.index.astype(int))
nat_cases

output: [1008, 842, 676, 650, 688, 689, 829, 690, 751, 3142, 850,
		 3216, 66, 287, 288, 289]
```

For most of the cases the error stemmed from an uncertainty in the actual date. One of many unfortunate facts about homicides is we don't always know when they occur. So the police officers who created this data provided a range of dates. For the purposes of my analysis I took the initial date to be the incident date. For cases that had an unknown incident date, I dropped the row.  

```python
# data frame of cases that have Not a date Time values
nat_case_df = case_df[case_df['Case number'].isin(nat_cases)].copy()
# 6/21/2008 - 8/7/2009 (Approximate) 	
nat_column = nat_case_df['Incident date'].str.split(pat='-', expand=True)
# drop '- | 8/7/2009' columns
nat_column.drop(labels=1, axis=1, inplace=True)
nat_column.rename(columns={0:'Incident date'}, inplace=True)
```

## GIS data
  The lasts bit of data that I wanted for my analysis was GPS coordinates. The original data provided street addresses but not GPS coordinates so my plan to map out all of the unsolved cases had to wait until I had latitude and longitudes for each case. There are a number of services that will let you batch convert addresses to coordinates but most of them cost money and I didn't want to spend it. So again, I manually collected GPS coordinates  for each case and created a * .csv file with three columns  'case', 'latitude', ' longitude'.   
## Preparing for Analysis 
  Thankfully the data I had was fairly complete. Apart from age and GPS coordinates I had most of the data I needed. I did have to fight with formatting and data types quite a bit but that's part of the process. I'm still not sure if my decision to treat the start of a date range as the incident date is the best solution but based on the information I have I think it's a reasonable one. Out of the 478 starting rows I only had to drop 3 due to lack of information. The next stage I'll be writing about is my analysis of the data. I take a look at the age distribution of the people murdered, the time distributions of the cases, and I use [folium](https://python-visualization.github.io/folium/) to examine the spatial distribution.