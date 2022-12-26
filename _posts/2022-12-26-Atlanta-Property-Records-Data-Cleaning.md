---
title: Atlanta Property Records Data Cleaning   
date: 2022-12-26  
categories: [Data Science,  GIS, Pandas, QGIS, Socioeconomics]  
permalink: /Atlanta-Property-Record-Cleaning/  

---
# Cleaning the data  
## Background  
   I left off the last post in this series talking about creating maps in QGIS. I preformed some basic analysis based on the demographic information that was available via the Fulton County government website. This post is going to pick up where that one left off. I won't be showing any maps here but I will be cleaning up 12 years of property records data and doing a little exploring with you.  

## Initializing  
   As with my last project I'll be using python and pandas v1.4.3. After this project I'll be updating both my pandas and python versions because I am woefully behind on my updates. As with the map I made in the first post the data I will be analyzing was sourced from the Fulton County government website. I am also using data from the Georgia Geospatial information office. I collected data for property records between 2009 and 2022. From here I loaded them into python and got to exploring.  
   
## Exploring  
  I wasn't exactly sure what the best way to tackle this was so I loaded a couple years worth of data into dataframes and looked around a bit. Each dataset had 33 columns and around 350-400k rows. It took me a little bit of reading to figure out what information I had and which columns it was stored in. 
  ```python  
Index(['Address', 'TAXPIN', 'ParcelID', 'ATRPIN', 'TaxDist', 'Owner', 'OWNER2',
'ADD2', 'ADD3', 'ADD4', 'ADD5', 'LUCode', 'NbrHood', 'PropClass',
'CLASS', 'TotAppr', 'TotAssess', 'IMPR_APPR', 'LandAppr', 'FUL_EX_COD',
'StructFlr', 'StructYr', 'TIEBACK', 'TaxYear', 'STATUS_COD', 'LivUnits',
'PCODE', 'UNIT_NUM', 'EXTVER', 'Shape__Area', 'Shape__Length'],
dtype='object')
  ```  
Most of these columns are fairly self explanatory but I was a bit confused by the multiple addresses listed. This ended up being for parcels where the owner had multiple addresses or a different business address in the case of rental properties. I did some more exploring to see which columns had enough data for me to use across multiple years. While doing this I discovered that the column names were changed in 2011 so before I got too far into my analysis I needed to unify the column names across the years.   
```python
# column names changed in 2011, renaming to make analysis easier. I wish I could just use pd.str functions :/
new_cols = {'PARID': 'ParcelID', 'TAX_DISTR': 'TaxDist','LIV_UNITS': 'LivUnits', 'OWNER1': 'Owner', 'STRUCT_YR': 'StructYr',
            'TOT_ASSESS': 'TotAssess','TOT_APPR': 'TotAppr', 'TAXYEAR': 'TaxYear','NBHD' : 'NbrHood', 'LUC':'LUCode',
           'LAND_APPR' : 'LandAppr', 'PROP_CLASS': 'PropClass', 'STRUCT_FLR': 'StructFlr', 'SITUS':'Address' }
Parcels_10.rename(columns=new_cols, inplace=True)
Parcels_09.rename(columns=new_cols, inplace=True)  
```  
  From here I did some cleaning to make sure the columns that I was interested in had enough data to analyze. I didn't want to get deep into my analysis only to discover that the parcel I was looking at didn't have enough information across the columns to actually look at.
```python
# drop useless columns or duplicate data
dup_col = ['DIGEST', 'VAL_ACRES', 'TAXPIN', 'ATRPIN']
Parcels_09.drop(dup_col, axis='columns', inplace=True)
Parcels_10.drop(dup_col, axis='columns', inplace=True)
# drop rows with no ['TaxYear'] data
dfs = [Parcels_09, Parcels_10, Parcels_11, Parcels_12, Parcels_13, Parcels_14, Parcels_15
        , Parcels_16, Parcels_17, Parcels_18, Parcels_19, Parcels_20, Parcels_21, Parcels_22]
# Check columns of interest for NaN values, and drop that row
for df in dfs:
        df.dropna(subset=['TaxYear', 'ParcelID', 'TaxDist', 'TotAppr', 'TotAssess', 'LivUnits'], inplace=True)
        df.astype({'TaxYear': 'int16'})
# check that it worked
Parcels_09[pd.isna(Parcels_09['TaxYear'])==True]
```  
  The caveat with this step is that it doesn't look across years. So I'll have to be mindful that when I'm comparing parcel numbers across the years I may have removed them due to lack of information in one year but not another. I'm a bit torn on how to handle this. Obivously it'll be easier to have uniform data across the years but as I throw out data I'm losing parts of the story. I may not be able to directly compare all properties across the years but within a year I think there will still be valuable information. So for now, I'll deal with the hazard of having  non-uniform parcel information.  
  With most of the cleaning done it was time to figure out how I wanted to stage things for analysis. I could leave each year as it's own dataframe or I could concatonate them into one massive dataframe and use the indexing techniques that pandas offers to manage the data. This was also a bit of a struggle for me. I wasn't sure what was the best way to do it or if there was a preferred way to do it. In the end I decided to concatonate everything into one gigantic dataframe with 13 years of data in it. This is a bit cumbersome and I need to be careful what operations I preform on it because 4.8M rows x 33 columns is pretty ungangly for my aging computer. However using one large dataframe allows me to use built in functions instead of clumsily writing my own loops to compare across 13 separate datafames. I won't save any noticeable amount of time in terms of selecting by years but since the built in functions aren't written in python I expect to gain some preformance here compared to for loops.  
  I did make the minor mistake and tried to check the whole dataset for nan values just to make sure I hadn't missed anything and while the dataframe only takes about 2 GB of RAM, preforming 
```python
   all_property_data[~all_property_data.isna()]
```  
ground my system to a crawl for a couple minutes. However narrowing the data down to only the columns I was interested in and then down to tax districts that were within the City of Atlanta only took a couple seconds. I also set some datatypes to help streamline memory usage.
```python
# pulls out just columns with useful amounts of or potentially interesting data
useful_df = all_property_data[['TaxYear', 'Owner', 'ParcelID', 'Address', 'AddrStreet','TaxDist', 'LUCode', 'NbrHood',
                                'TotAppr', 'TotAssess', 'LandAppr', 'LandAssess', 'LivUnits', 'LandAcres']] 
# a list of parcel tax IDs in the City of Atlanta.
atlanta_tax_IDs = ['05', '05A', '05B', '05C', '05D', '05E', '05F', '05G', '05H', '05I', '05J',
                    '05K', '05L', '05P', '05Q', '05R', '05S', '05T', '05U', '05V', '05W', '05X',
                  '05Y', '05Z'] 
# fixes data types to pare down memory usage
useful_df = useful_df.astype({'TaxYear' : 'int16', 'TotAppr' : 'int32', 'TotAssess' : 'int32', 'LandAppr' : 'int32'}) 
# Selects just data that has Atlanta tax IDs
atlanta_df = useful_df[useful_df['TaxDist'].isin(atlanta_tax_IDs)]
```   
## Wrapping up  
  That's pretty much it! It took me way to long to get this done and even longer to get a little post written up about it but it's finally done. Now I can clear out the other DataFrames from memory and keep just atlanta_df in RAM, begin looking at some sales patterns, and changes of property value across the years. Thanks for sticking with me. 
   
## Data sources  
#### State wide  
- [Population Change](https://opendata.atlantaregional.com/datasets/GARC::population-change-1990-2020-by-zip-code-1/explore?location=33.255502%2C-84.052413%2C9.58&showTable=true)  
#### Fulton County    
- [Fulton County Tax Districts](https://fultonassessor.org/wp-content/uploads/sites/16/2018/05/Fulton-County-Tax-Districts-2018.pdf)  
- https://www.fultoncountyga.gov/maps   
- http://share.myfultoncountyga.us/datashare/fultoncounty/data/Demog_CensusTracts2010Profile/SHP/Demog_CensusTracts2010Profile.zip  
- [Georgia Geospatial Information Office](https://gio.ga.gov/)  
- [Georgia Association of Regional Commissions](https://opendata.atlantaregional.com/datasets/GARC::major-roads/explore?location=33.706256%2C-84.434300%2C9.53)  