# carbon_emisions
## 1. Introduction

This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

![image](https://github.com/user-attachments/assets/73a8cca6-dc21-4517-8cb9-384ee8a120ce)

### 1.1 Data model
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.

![image](https://github.com/user-attachments/assets/fa9bc8f7-6027-4d6f-b1a4-f7df14fb2c85)

## 1.2 Data Structure
Table 'product_emissions'
```sql
SELECT * FROM product_emissions pe LIMIT 10;
```
|id|company_id|country_id|industry_group_id|year|product_name|weight_kg|carbon_footprint_pcf|upstream_percent_total_pcf|operations_percent_total_pcf|downstream_percent_total_pcf|
|--|----------|----------|-----------------|----|------------|---------|--------------------|--------------------------|----------------------------|----------------------------|
|10056-1-2014|82|28|2|2014|Frosted Flakes(R) Cereal|0.7485|2|57.50|30.00|12.50|
|10056-1-2015|82|28|15|2015|"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"|0.7485|2|57.50|30.00|12.50|
|10222-1-2013|83|28|8|2013|Office Chair|20.68|73|80.63|17.36|2.01|
|10261-1-2017|14|16|25|2017|Multifunction Printers|110.0|1488|30.65|5.51|63.84|
|10261-2-2017|14|16|25|2017|Multifunction Printers|110.0|1818|25.08|4.51|70.41|
|10261-3-2017|14|16|25|2017|Multifunction Printers|110.0|2274|20.05|3.61|76.34|
|10324-1-2016|15|16|19|2016|KURALON  fiber|1500.0|10000|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|
|10418-1-2013|84|9|19|2013|Portland Cement|1000.0|1102|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|
|10661-10-2014|85|28|11|2014|Regular Straight 505® Jeans – Steel (Water<Less™)|0.7665|15|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|
|10661-10-2015|85|28|6|2015|Regular Straight 505® Jeans – Steel (Water<Less™)|0.7665|15|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|

Table 'industry_groups'
```sql
SELECT * FROM industry_groups ig  LIMIT 10;
```
|id|industry_group|
|--|--------------|
|1|"Consumer Durables, Household and Personal Products"|
|2|"Food, Beverage & Tobacco"|
|3|"Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber"|
|4|"Mining - Iron, Aluminum, Other Metals"|
|5|"Pharmaceuticals, Biotechnology & Life Sciences"|
|6|"Textiles, Apparel, Footwear and Luxury Goods"|
|7|Automobiles & Components|
|8|Capital Goods|
|9|Chemicals|
|10|Commercial & Professional Services|

Table 'companies'
```sql
SELECT * FROM companies c  LIMIT 10;
```
|id|company_name|
|--|------------|
|1|"Autodesk, Inc."|
|2|"Casio Computer Co., Ltd."|
|3|"Cisco Systems, Inc."|
|4|"CNX Coal Resources, LP"|
|5|"Coca-Cola Enterprises, Inc."|
|6|"Compañía Española de Petróleos, S.A.U. CEPSA"|
|7|"Daikin Industries, Ltd."|
|8|"Elitegroup computer systems co., Ltd."|
|9|"Fuji Xerox Co., Ltd."|
|10|"Gamesa Corporación Tecnológica, S.A."|

Table 'countries'
```sql
SELECT * FROM countries c   LIMIT 10;
```
|id|country_name|
|--|------------|
|1|Australia|
|2|Belgium|
|3|Brazil|
|4|Canada|
|5|Chile|
|6|China|
|7|Colombia|
|8|Finland|
|9|France|
|10|Germany|

## 2. Data Explore
Data duplicate
```sql
SELECT *,
	COUNT(*) AS duplicate_count
FROM product_emissions pe 
GROUP BY 
	id,
	company_id,
	country_id,
	industry_group_id,
	year,
	product_name,
	weight_kg ,
	carbon_footprint_pcf ,
	upstream_percent_total_pcf ,
	operations_percent_total_pcf ,
	downstream_percent_total_pcf 
HAVING COUNT(*) > 1
LIMIT 10;
```
|id|company_id|country_id|industry_group_id|year|product_name|weight_kg|carbon_footprint_pcf|upstream_percent_total_pcf|operations_percent_total_pcf|downstream_percent_total_pcf|duplicate_count|
|--|----------|----------|-----------------|----|------------|---------|--------------------|--------------------------|----------------------------|----------------------------|---------------|
|10056-1-2014|82|28|2|2014|Frosted Flakes(R) Cereal|0.7485|2|57.50|30.00|12.50|2|
|10056-1-2015|82|28|15|2015|"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"|0.7485|2|57.50|30.00|12.50|2|
|10222-1-2013|83|28|8|2013|Office Chair|20.68|73|80.63|17.36|2.01|2|
|10261-1-2017|14|16|25|2017|Multifunction Printers|110.0|1488|30.65|5.51|63.84|2|
|10261-2-2017|14|16|25|2017|Multifunction Printers|110.0|1818|25.08|4.51|70.41|2|
|10261-3-2017|14|16|25|2017|Multifunction Printers|110.0|2274|20.05|3.61|76.34|2|
|10324-1-2016|15|16|19|2016|KURALON  fiber|1500.0|10000|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10418-1-2013|84|9|19|2013|Portland Cement|1000.0|1102|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-1-2014|85|28|11|2014|501® Original Jeans – Dark Stonewash|0.997|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|
|10661-1-2015|85|28|6|2015|501® Original Jeans – Dark Stonewash|0.997|16|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|N/a (product with insufficient stage-level data)|2|

Duplicate result
```sql
SELECT COUNT(product_name) AS 'Total number of product',
       COUNT(DISTINCT(product_name)) AS 'Number of unique product'
FROM product_emissions;
```
|Total number of product|Number of unique product|
|-----------------------|------------------------|
|1037|661|

## 3. Data Analysis
### 3.1. Which products contribute the most to carbon emissions ?

```SQL
SELECT product_name, ROUND(carbon_footprint_pcf,2) AS 'Average'  FROM product_emissions pe 
GROUP BY product_name 
ORDER BY carbon_footprint_pcf DESC
LIMIT 10;
```
|product_name|Average|
|------------|-------|
|Wind Turbine G128 5 Megawats|3718044|
|Wind Turbine G132 5 Megawats|3276187|
|Wind Turbine G114 2 Megawats|1532608|
|Wind Turbine G90 2 Megawats|1251625|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687|
|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|167000|
|TCDE|99075|
|Mercedes-Benz GLE (GLE 500 4MATIC)|91000|
|Mercedes-Benz S-Class (S 500)|85000|
|Mercedes-Benz SL (SL 350)|72000|

### Conclusion:
Among the analyzed products, Wind Turbines dominate the list with the highest average carbon footprints. The Wind Turbine G128 5 Megawatts tops the chart with an average emission of over 3.7 million units, followed by the G132 and G114 models. These figures reflect the substantial environmental cost associated with manufacturing and deploying large-scale renewable energy infrastructure. In contrast, vehicles like the Mercedes-Benz GLE and S-Class, as well as construction-related products, have significantly lower average emissions, indicating a wide variance in carbon footprints across different product categories.



### 3.2. What are the industry groups of these products ?
```sql
SELECT 
    pe.product_name, 
    ROUND(pe.carbon_footprint_pcf, 2) AS Average,  
    ig.industry_group
FROM product_emissions pe 
JOIN industry_groups ig 
    ON pe.industry_group_id = ig.id  
GROUP BY pe.product_name, ig.industry_group
ORDER BY pe.carbon_footprint_pcf DESC
LIMIT 10;
```
|product_name|Average|industry_group|
|------------|-------|--------------|
|Wind Turbine G128 5 Megawats|3718044|Electrical Equipment and Machinery|
|Wind Turbine G132 5 Megawats|3276187|Electrical Equipment and Machinery|
|Wind Turbine G114 2 Megawats|1532608|Electrical Equipment and Machinery|
|Wind Turbine G90 2 Megawats|1251625|Electrical Equipment and Machinery|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|191687|Automobiles & Components|
|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|167000|Materials|
|TCDE|99075|Materials|
|Mercedes-Benz GLE (GLE 500 4MATIC)|91000|Automobiles & Components|
|Mercedes-Benz S-Class (S 500)|85000|Automobiles & Components|
|Mercedes-Benz SL (SL 350)|72000|Automobiles & Components|

### Conclusion:
The products analyzed span multiple industry groups, with a heavy concentration in Electrical Equipment and Machinery, particularly Wind Turbines, which dominate the highest carbon footprints. The Automobiles & Components sector also contributes significantly, with several Mercedes-Benz models and other vehicle types like the Land Cruiser. The Materials industry, including products like Retaining wall structures and TCDE, shows a moderate level of emissions. This distribution highlights the major environmental impact of industries involved in heavy machinery, vehicles, and construction materials.

### 3.3 What are the industries with the highest contribution to carbon emissions ?
```SQL
SELECT 
    ig.industry_group,
    ROUND(SUM(pe.carbon_footprint_pcf), 2) AS 'Total PCF'
FROM product_emissions pe
JOIN industry_groups ig ON ig.id = pe.industry_group_id
GROUP BY ig.industry_group
ORDER BY ROUND(SUM(pe.carbon_footprint_pcf), 2) DESC
LIMIT 10;
```
|industry_group|Total PCF|
|--------------|---------|
|Electrical Equipment and Machinery|9801558.00|
|Automobiles & Components|2582264.00|
|Materials|577595.00|
|Technology Hardware & Equipment|363776.00|
|Capital Goods|258712.00|
|"Food, Beverage & Tobacco"|111131.00|
|"Pharmaceuticals, Biotechnology & Life Sciences"|72486.00|
|Chemicals|62369.00|
|Software & Services|46544.00|
|Media|23017.00|

### Conclusion:
The Electrical Equipment and Machinery sector is the largest contributor to carbon emissions in the dataset, with a total of approximately 9.8 million units, far exceeding any other industry group. Following that, the Automobiles & Components industry comes in second with 2.58 million units, reflecting the high carbon cost of manufacturing vehicles and components. The Materials and Technology Hardware & Equipment sectors show emissions of 577,595 and 363,776 units respectively, which are significant but much lower than the leading sectors. Other industries, such as Food, Beverage & Tobacco, Pharmaceuticals, and Software & Services, contribute relatively smaller amounts, with emissions ranging from 23,000 to 111,000 units. This data suggests that high-emission industries like Electrical Equipment and Automobiles dominate the total carbon footprint, while industries like Software & Services and Media are less impactful in terms of carbon emissions.

### 3.4. What are the companies with the highest contribution to carbon emissions ?
```SQL
SELECT 
    cm.company_name ,
    ROUND(SUM(pe.carbon_footprint_pcf), 2) AS 'Total'
FROM product_emissions pe
JOIN companies cm ON cm.id = pe.company_id 
GROUP BY cm.company_name
ORDER BY Total DESC
LIMIT 10;
```

|company_name|Total|
|------------|-----|
|"Gamesa Corporación Tecnológica, S.A."|9778464.00|
|Daimler AG|1594300.00|
|Volkswagen AG|655960.00|
|"Mitsubishi Gas Chemical Company, Inc."|212016.00|
|"Hino Motors, Ltd."|191687.00|
|Arcelor Mittal|167007.00|
|Weg S/A|160655.00|
|General Motors Company|137007.00|
|"Lexmark International, Inc."|132012.00|
|"Daikin Industries, Ltd."|105600.00|

### Conclusion:
From the dataset, Gamesa Corporación Tecnológica, S.A. emerges as the top emitter, with a staggering total of approximately 9.78 million units, far surpassing all other companies. The next highest contributors, such as Daimler AG and Volkswagen AG, report emissions of around 1.59 million and 656,000 units respectively—still significant but considerably lower than Gamesa. This sharp gap highlights the dominant carbon footprint associated with Gamesa, likely due to the scale or nature of its operations. Other companies in the top 10, including Mitsubishi Gas Chemical, General Motors, and Daikin Industries, show relatively moderate emissions. The data reflects a strong concentration of emissions among a few large manufacturers, suggesting targeted opportunities for corporate sustainability improvements.

### 3.5. What are the countries with the highest contribution to carbon emissions ?
```SQL
SELECT 
    cm.country_name ,
    ROUND(SUM(pe.carbon_footprint_pcf), 2) AS 'Total'
FROM product_emissions pe
JOIN countries cm ON cm.id = pe.company_id 
GROUP BY cm.country_name
ORDER BY Total DESC
LIMIT 10;
```
|country_name|Total|
|------------|-----|
|Germany|9778464.00|
|Lithuania|212016.00|
|Greece|191687.00|
|Japan|132012.00|
|Colombia|105600.00|
|South Africa|35505.00|
|France|21364.00|
|Italy|20000.00|
|Ireland|11160.00|
|India|9328.00|

### Conclusion:
The data indicates that Germany is by far the largest contributor to carbon emissions, with a total of approximately 9.78 million units, making it a significant outlier compared to other countries. The next highest contributors — Lithuania and Greece — have emissions of just over 200,000 and 190,000 units respectively, which are only small fractions of Germany's total. The rest of the countries in the top 10, including Japan, Colombia, and South Africa, show much lower emission levels, gradually decreasing to India at around 9,300 units. This stark contrast suggests that Germany plays a disproportionately large role in overall emissions within the dataset, possibly due to industrial scale, product types, or reporting volume.

### 3.6. What is the trend of carbon footprints (PCFs) over the years ?
```SQL
SELECT  year, SUM(carbon_footprint_pcf) AS 'Total_PCFs_OVER_YEARS'
 FROM  product_emissions 
 GROUP BY year
ORDER BY Total_PCFs_OVER_YEARS DESC;
```
|year|Total_PCFs_OVER_YEARS|
|----|---------------------|
|2015|10840415|
|2016|1640182|
|2014|624226|
|2013|503857|
|2017|340271|

### Conclusion:
The data reveals a sharp fluctuation in carbon footprints over the years. The year 2015 recorded the highest total PCFs, reaching over 10.8 million units, which is significantly higher than all other years. This spike is followed by a dramatic drop in 2016 to around 1.64 million, and an even lower trend continues in the surrounding years. The earlier years, such as 2013 and 2014, show relatively moderate emissions, while 2017 marks the lowest total in the dataset. This trend may suggest either a major emission event in 2015, improved sustainability practices after that year, or potential gaps in data collection across different years.

### 3.7. Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time ?

```SQL
SELECT 
    ig.industry_group AS 'Industry Group',
    ROUND(SUM(CASE WHEN pe.year = 2013 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2013 Emission',
    ROUND(SUM(CASE WHEN pe.year = 2014 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2014 Emission',
    ROUND(SUM(CASE WHEN pe.year = 2015 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2015 Emission',
    ROUND(SUM(CASE WHEN pe.year = 2016 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2016 Emission',
    ROUND(SUM(CASE WHEN pe.year = 2017 THEN pe.carbon_footprint_pcf ELSE 0 END), 2) AS '2017 Emission'
FROM product_emissions AS pe
LEFT JOIN industry_groups AS ig ON ig.id = pe.industry_group_id
GROUP BY ig.industry_group
ORDER BY 
    `2017 Emission` ,
    `2016 Emission` ,
    `2015 Emission` ,
    `2014 Emission` ,
    `2013 Emission`;
```
|Industry Group|2013 Emission|2014 Emission|2015 Emission|2016 Emission|2017 Emission|
|--------------|-------------|-------------|-------------|-------------|-------------|
|Household & Personal Products|0.00|0.00|0.00|0.00|0.00|
|"Pharmaceuticals, Biotechnology & Life Sciences"|32271.00|40215.00|0.00|0.00|0.00|
|Tobacco|0.00|0.00|1.00|0.00|0.00|
|Semiconductors & Semiconductors Equipment|0.00|0.00|3.00|0.00|0.00|
|Retailing|0.00|19.00|11.00|0.00|0.00|
|Gas Utilities|0.00|0.00|122.00|0.00|0.00|
|Food & Beverage Processing|0.00|0.00|141.00|0.00|0.00|
|Telecommunication Services|52.00|183.00|183.00|0.00|0.00|
|Trading Companies & Distributors and Commercial Services & Supplies|0.00|0.00|239.00|0.00|0.00|
|"Textiles, Apparel, Footwear and Luxury Goods"|0.00|0.00|387.00|0.00|0.00|
|"Consumer Durables, Household and Personal Products"|0.00|0.00|931.00|0.00|0.00|
|Tires|0.00|0.00|2022.00|0.00|0.00|
|Containers & Packaging|0.00|0.00|2988.00|0.00|0.00|
|"Mining - Iron, Aluminum, Other Metals"|0.00|0.00|8181.00|0.00|0.00|
|"Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber"|0.00|0.00|8909.00|0.00|0.00|
|Chemicals|0.00|0.00|62369.00|0.00|0.00|
|Electrical Equipment and Machinery|0.00|0.00|9801558.00|0.00|0.00|
|Food & Staples Retailing|0.00|773.00|706.00|2.00|0.00|
|Semiconductors & Semiconductor Equipment|0.00|50.00|0.00|4.00|0.00|
|Utilities|122.00|0.00|0.00|122.00|0.00|
|Consumer Durables & Apparel|2867.00|3280.00|0.00|1162.00|0.00|
|Media|9645.00|9645.00|1919.00|1808.00|0.00|
|Energy|750.00|0.00|0.00|10024.00|0.00|
|Automobiles & Components|130189.00|230015.00|817227.00|1404833.00|0.00|
|Software & Services|6.00|146.00|22856.00|22846.00|690.00|
|Commercial & Professional Services|1157.00|477.00|0.00|2890.00|741.00|
|"Food, Beverage & Tobacco"|4995.00|2685.00|0.00|100289.00|3162.00|
|Technology Hardware & Equipment|61100.00|167361.00|106157.00|1566.00|27592.00|
|Capital Goods|60190.00|93699.00|3505.00|6369.00|94949.00|
|Materials|200513.00|75678.00|0.00|88267.00|213137.00|

### Conclusion:
The industry with the highest carbon footprint in 2015 is Electrical Equipment and Machinery, recording over 9.8 million PCFs, a significant outlier compared to other industries. Emissions in general tend to peak in 2015 across many sectors. Notably, Automobiles & Components and Materials also show substantial contributions over multiple years, particularly in 2016 and 2017 respectively. Meanwhile, several industries have minimal or no emissions across all years, indicating either limited data or lower carbon output.

## Overall Analysis:
The products with the highest carbon emissions are typically linked to heavy industries. Notably, several automobile models — including the Land Cruiser Prado, Mercedes-Benz GLA, S-Class, and SL — are among the top emitters during production.
Interestingly, the Pharmaceuticals, Biotechnology & Life Sciences sector ranks 7th in total emissions, reminding us that even our pursuit of health has an environmental cost.






