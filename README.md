# carbon_emisions


This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.
![image](https://github.com/user-attachments/assets/73a8cca6-dc21-4517-8cb9-384ee8a120ce)

### 1.1 Data model
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.
https://lms.swisscoding.edu.vn/pluginfile.php/19033/mod_label/intro/Database%20diagram.png

## 1.2 Data Structure
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.







```SQL
SELECT product_name, ROUND(carbon_footprint_pcf,2) AS 'Average'  FROM product_emissions pe 
GROUP BY product_name 
ORDER BY carbon_footprint_pcf DESC
LIMIT 10;
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
```

```SQL
SELECT 
    pe.product_name, 
    ROUND(pe.carbon_footprint_pcf, 2) AS Average,  
    ig.industry_group
FROM product_emissions pe 
JOIN industry_groups ig 
    ON pe.industry_group_id = ig.id  -- JOIN theo industry_group_id
GROUP BY pe.product_name, ig.industry_group
ORDER BY pe.carbon_footprint_pcf DESC
LIMIT 10;
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
```
```SQL
SELECT 
    ig.industry_group,
    ROUND(SUM(pe.carbon_footprint_pcf), 2) AS 'Total PCF'
FROM product_emissions pe
JOIN industry_groups ig ON ig.id = pe.industry_group_id
GROUP BY ig.industry_group
ORDER BY ROUND(SUM(pe.carbon_footprint_pcf), 2) DESC
LIMIT 10;
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
```
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






