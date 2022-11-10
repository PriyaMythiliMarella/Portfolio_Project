![image](https://user-images.githubusercontent.com/83623143/201114333-fa5f59fc-e7bb-4463-aed5-69232fc9839b.png)
![image](https://user-images.githubusercontent.com/83623143/201115423-30e7ab20-80c4-45be-81f0-cf43b3ab389e.png)
![image](https://user-images.githubusercontent.com/83623143/201114333-fa5f59fc-e7bb-4463-aed5-69232fc9839b.png)
![image](https://user-images.githubusercontent.com/83623143/201115423-30e7ab20-80c4-45be-81f0-cf43b3ab389e.png)

Queries for data validation, cleaning & manipulation of climate change

-- selecting temp table

select * from [Portfoio_project].[dbo].[Temp$]
where country is not null
order by country, dt

--trying to alter dt to int but there was a error

alter table [Portfoio_project].[dbo].[Temp$]
alter column dt int

-- using cast to to convert datatype to date

select cast([dt] as date) from [Portfoio_project].[dbo].[Temp$] as Date

-- creating new tabe to make it simple as Table_1

select CAST(dt as date) as Date, AverageTemperature, AverageTemperatureUncertainty, Country into Table_1
from [Portfoio_project].[dbo].[Temp$] 


select * from Table_1

--using format to select only year from the date into temp_table

select FORMAT(Date, 'yyyy') as Year, AverageTemperature, AverageTemperatureUncertainty, 
Country into temp_table from Table_1
where country is not null
group by country
order by country

alter table Table_1
alter column Date date


select CAST(Date as date)
format(Date, 'yyyy') from Table_1

select FORMAT(Date, 'yyyy') as Year from Table_1 

-- creating table as temperature by using aggregate and group by functions

select count(Year) as Count_Year, AVG(AverageTemperature) as Avg_Temp, Avg(AverageTemperatureUncertainty) as Avg_Temp_Uncertainty, 
Country into temperature from temp_table
group by Country
order by Country

-- checking the data of united states

select * from temperature
where Country = 'United States'

select * from temperature
order by Country

 ![image](https://user-images.githubusercontent.com/83623143/201115878-247d3917-c0e0-4b5f-9520-ceb29a05f040.png)



1365	5.66115395894428	0.463844574780059	Ã…land
1365	14.225853372434	0.494984604105572	Afghanistan
1365	24.2422653958944	0.235238269794721	Africa
1365	12.8859809384164	0.431643695014663	Albania
1365	23.2662118768328	0.517530058651026	Algeria
1365	26.7195095307918	0.483642228739003	American Samoa
1365	11.462202346041	0.369406158357771	Andorra
1365	21.962348973607	0.5805784457478	Angola
1365	26.8114164222874	0.37850219941349	Anguilla


Now two tables are prepared as Crops_Table and temperature
Joining the two table where country is common to find the relation between temperature and crops
--


