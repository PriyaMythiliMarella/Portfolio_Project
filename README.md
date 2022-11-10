Relation between global crop production, yield and temperature (using excel, sql, tableau)

The aim of this project is to showcase the skills learned by asking SMART questions, structured thinking, data management, data cleaning, data analysis & data visualization.
About the Project
Here we are going to see the relation between global crop production, crop yield, crop production in hectares of land and global temperature from 1900 – 2017

Twentieth Century Crop Statistics, v1 (1900 – 2017)

Data collected from SOCIOECONOMIC DATA AND APPLICATIONS CENTER (SEDAC) A Data Center in NASA's Earth Observing System Data and Information System (EOSDIS) — Hosted by CIESIN at Columbia University
 https://sedac.ciesin.columbia.edu/data/set/food-twentieth-century-crop-statistics-1900-2017
This document outlines the methodology and data sets used to construct the Twentieth Century Crop Statistics, 1900-2017, and consists of national or subnational maize and wheat production, yield, and harvested area statistics for all available years from 1900- 2017.
Climate Change: Earth Surface Temperature Data
Global land temperature data collected from kaggle website https://www.kaggle.com/datasets/berkeleyearth/climate-change-earth-surface-temperature-data
Here it contains
Global Land and Ocean-and-Land Temperatures (GlobalTemperatures.csv):
•	Date: starts in 1750 for average land temperature and 1850 for max and min land temperatures and global ocean and land temperatures
•	LandAverageTemperature: global average land temperature in celsius
•	LandAverageTemperatureUncertainty: the 95% confidence interval around the average


Process
In the process phase is where we create and transform data but still maintaning its integrity. It is also where we test & validate data that can help us determine which data fields to clean if necessary
In this phase is where I:
•	Data is collected in excel and done possible cleaning here itself
•	used Microsoft SQL server management studio as an analysis tool, since it processes data faster compared to spreadsheets for big datasets.
•	checked for data integrity by running different queries across field names that would help me in data cleaning.
•	transformed data by adding and updating those columns that will add other categories to group user types even more to uncover patterns or give more context.
•	verified and documented data cleaning results.
Data Cleaning and Manipulation Documentation
Data manipulation Using Spreadsheet
As there are many sheets and I decided to take only important sheets and renamed the sheets that I am using.
Data validation using SQL (BigQuery)
After collecting necessary sheets I have imported data to SQL using Import data function.
Here I
1.	Checked for NULL values for each column / data field.
2.	Checked for duplicate entries.
3.	Checked for misspelled words for columns with STRING as its data type.
4.	Checked for trailing & leading spaces on STRING values
5.	Checked for data types that are used
Commands used: SELECT, FROM, WHERE, GROUP BY, ORDER BY, COUNT, IS NULL, OR & LIKE
Queries for data validation, cleaning & manipulation of Crop statistics

select * from [Portfoio_project].[dbo].[Sheet1$]

--checking the columns present in data

select admin0 from [Portfoio_project].[dbo].[Sheet1$]

select admin0, count(Harvest_year) as year from [Portfoio_project].[dbo].[Sheet1$] 
WHERE admin0 IS NOT NULL
group by admin0
order by year

select admin0, count(Harvest_year) as year, count(crop) as crop_count from [Portfoio_project].[dbo].[Sheet1$]
WHERE admin0 IS NOT NULL
group by admin0
order by year

-- checking and selecting the necessary columns by removing nulls 

select admin0, Harvest_year, crop, [hectares (ha)], [production (tonnes)], [yield(tonnes/ha)] from [Portfoio_project].[dbo].[Sheet1$]
where [hectares (ha)] is not null
and [yield(tonnes/ha)] is not null
and [production (tonnes)] is not null
order by admin0, Harvest_year

--checking the data for particular country

select admin0, Harvest_year, crop, [hectares (ha)], [production (tonnes)], [yield(tonnes/ha)] from [Portfoio_project].[dbo].[Sheet1$]
where [hectares (ha)] is not null
and [yield(tonnes/ha)] is not null
and [production (tonnes)] is not null
and admin0 = 'India'

-- selecting unique countries

SELECT distinct admin0 FROM [Portfoio_project].[dbo].[Sheet1$]

select admin0, Avg([hectares (ha)]), AVG([production (tonnes)]), Avg([yield(tonnes/ha)]) from [Portfoio_project].[dbo].[Sheet1$]
where [hectares (ha)] is not null
and [yield(tonnes/ha)] is not null
and [production (tonnes)] is not null
--group by admin0
order by admin0, Harvest_year

select Avg([yield(tonnes/ha)]) from [Portfoio_project].dbo.Sheet1$

-- changing hectares column data type from varchar to bigint

alter table [Portfoio_project].[dbo].[Sheet1$]
alter column [hectares (ha)] money

SELECT CONVERT(int, '[hectares (ha)]') from [Portfoio_project].[dbo].[Sheet1$]
go

select admin0, Harvest_year, [hectares (ha)] from [Portfoio_project].[dbo].[Sheet1$]
where [hectares (ha)] = '2101756.9'

SELECT CONVERT(money, '2101756.9')

alter table [Portfoio_project].[dbo].[Sheet1$]
alter column [hectares (ha)] bigint

alter table [Portfoio_project].[dbo].[Sheet1$]
alter column [production (tonnes)] money

-- changing productin column data type from varchar to bigint


alter table [Portfoio_project].[dbo].[Sheet1$]
alter column [production (tonnes)] bigint

select Avg([hectares (ha)]) from [Portfoio_project].dbo.Sheet1$
where  [hectares (ha)] is not null

select Avg([production (tonnes)]) from [Portfoio_project].dbo.Sheet1$
where  [production (tonnes)] is not null

-- selecting necessary columnns by using aggregate and group by functions

select country, crop, sum(Harvest_year) as Sum_year, Avg([hectares (ha)]) as Avg_hectares, AVG([production (tonnes)]) as Avg_production, Avg([yield(tonnes/ha)]) as Avg_yield from [Portfoio_project].[dbo].[Sheet1$]
where [hectares (ha)] is not null
and [yield(tonnes/ha)] is not null
and [production (tonnes)] is not null
group by country, crop
order by country 

-- creating new table by grouping necessary columns

select country, crop, cast(sum(Harvest_year) as bigint) as Sum_year, cast(Avg([hectares (ha)]) as bigint) as Avg_hectares, 
cast(AVG([production (tonnes)]) as bigint) as Avg_production, Avg([yield(tonnes/ha)]) as Avg_yield 
into Table_1 from [Portfoio_project].[dbo].[Sheet1$]
where [hectares (ha)] is not null
and [yield(tonnes/ha)] is not null
and [production (tonnes)] is not null
group by country, crop
order by country 

select * from Table_1

-- changing the column name by using sp_rename changed admin0 to country

alter table 

sp_rename '[Portfoio_project].[dbo].[Sheet1$].admin0', 'country', 'column'

select * from Table_1
order by country

-- dropping table_1
drop table Table_1

-- creating new table as Crops_Table by grouping and aggregating necessary columns

select country, crop, count(Harvest_year) as count_Year, cast(Avg([hectares (ha)]) as bigint) as Avg_hectares, 
cast(AVG([production (tonnes)]) as bigint) as Avg_production, Avg([yield(tonnes/ha)]) as Avg_yield 
into Crops_Table from [Portfoio_project].[dbo].[Sheet1$]
where [hectares (ha)] is not null
and [yield(tonnes/ha)] is not null
and [production (tonnes)] is not null
group by  country, crop
order by country

select * from Crops_Table
order by country

![image](https://user-images.githubusercontent.com/83623143/201116342-048f328a-d1d4-4085-852c-5b95076dd97c.png)
