--See whole Data 
SELECT * FROM Census_Data_1
SELECT * FROM Census_Data_2

--Count of rows in Data
SELECT COUNT(*) FROM Census_Data_1
SELECT COUNT(*) FROM Census_Data_2

--Delete all values from Table 
TRUNCATE TABLE Census_Data_1

--Delete Table From Data_Base 
DROP TABLE Census_Data_1

--Data set for Karnataka and Kerala in Census_Data_1
SELECT * FROM Census_Data_1 WHERE State IN ('Karnataka' , 'Kerala')

--Total population in India in Census_Data_2
SELECT SUM(POPULATION) AS TOTAL_POPULATION FROM Census_Data_2

--Average Growth in India from Census_Data_1 in Percentage
SELECT FORMAT(AVG(Growth),'p') AS Avg_Growth from Census_Data_1

--Average Growth for each states from Census_Data_1 in Percentage
SELECT STATE,FORMAT(AVG(Growth),'p') AS Avg_Growth  from Census_Data_1  GROUP BY State;

--Average Sex_Ratio for each state sort by Descending 
SELECT STATE, ROUND(AVG(Sex_Ratio),0) AS Avg_Sex_Ratio  from Census_Data_1  GROUP BY State ORDER BY Avg_Sex_Ratio DESC;

--Average litracy rate by state which has literacy rate more than 90
SELECT STATE, ROUND(AVG(Literacy),0) AS Avg_Literacy_Rate  from Census_Data_1  GROUP BY State HAVING ROUND(AVG(Literacy),0)>90

--Top 3 litracy states by literacy
SELECT top 3 STATE, ROUND(AVG(Literacy),0) AS Avg_Literacy_Rate  from Census_Data_1  GROUP BY State ORDER BY Avg_Literacy_Rate DESC;

--Botom 3 litracy states by literacy
SELECT TOP 3 STATE, ROUND(AVG(Literacy),0) AS Avg_Literacy_Rate  from Census_Data_1  GROUP BY State ORDER BY Avg_Literacy_Rate 

--Select the states names starting with 'A'
SELECT DISTINCT State  FROM Census_Data_2 WHERE State LIKE 'A%' 

--Count of States in Census data
SELECT COUNT(DISTINCT(State)) AS COUNT_OF_STATES FROM Census_Data_2 


--All Top 3 states and Bottom 3 states in one table by creating 2 seperate temprary table and joining them
DROP TABLE IF EXISTS #Top_States
CREATE TABLE #Top_States ( State nvarchar(255), Top_States float)
INSERT INTO #Top_States
SELECT TOP 3 STATE, ROUND(AVG(Literacy),0) AS Avg_Literacy_Rate  from Census_Data_1  GROUP BY State ORDER BY ROUND(AVG(Literacy),0) DESC;

DROP TABLE IF EXISTS #Bottom_States
CREATE TABLE #Bottom_States ( State nvarchar(255), Top_States float)
INSERT INTO #Bottom_States
SELECT TOP 3 STATE, ROUND(AVG(Literacy),0) AS Avg_Literacy_Rate  from Census_Data_1  GROUP BY State ORDER BY ROUND(AVG(Literacy),0) ASC;

SELECT * FROM #Top_States 
UNION
SELECT * FROM #Bottom_States 
ORDER BY Top_States DESC


--Full Join both the tables with same districts 
SELECT a.state, a.District,a.Growth, a.Literacy, a.Sex_Ratio, B.Population ,B.Area_km2
FROM Census_Data_1 AS A
FULL JOIN Census_Data_2 AS B
ON A.District = B.District;


