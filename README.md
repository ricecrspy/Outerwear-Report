# 1.0 BERLIN MARKET ANALYSIS

#### 1.1 ABOUT
Google Data Analytics Capstone: Berlin, a premium streetwear fashion brand, wants to expand by offering outerwear pieces in its collection. However, before launching the new product line, the company's President wants a high-level report analyzing current market trends and competitors. The Director of Marketing and Sales tasked me with performing market research and creating a report that highlights trends, market insights, and key recommendations using data analytics.

#### 1.2 DATA STRUCTURES
I identified five e-commerce boutique stores that align with Berlin's market positioning in terms of price, product design, target customer, and quality. Using a Chrome browser web crawler extension, I scraped over 300 rows of data observations, about 30-70 rows from each website, based on the following variables: brand name, product description, colorway, price, discount, and sale price. Data structures varied from store to store, and as a result, a unique table was created for each, totaling five tables. <br></br>

To learn more about the data cleaning process [click here](data-cleaning-sql.md)

#### 1.3 KEY INDICATORS (DASHBOARD)
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus vulputate turpis non tristique rhoncus. Sed facilisis, nibh ac egestas tincidunt, orci massa hendrerit nisl, quis mollis lectus metus non dui. Aenean imperdiet magna in neque consectetur mattis. Integer faucibus tempor egestas. Duis ullamcorper eget nisl id malesuada. Quisque pulvinar justo vel rhoncus placerat. Curabitur ultricies facilisis ex semper ultrices. Vivamus vel dui vitae erat iaculis ultricies eu vel augue. Aliquam vel augue interdum, dignissim nisl at, tempor velit.
<br></br>
           • KPI 1 Description <br></br>
           • KP2 2 Description <br></br>

#### 1.4 MARKET SEGMENTATION
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus vulputate turpis non tristique rhoncus. Sed facilisis, nibh ac egestas tincidunt, orci massa hendrerit nisl, quis mollis lectus metus non dui. Aenean imperdiet magna in neque consectetur mattis. Integer faucibus tempor egestas. Duis ullamcorper eget nisl id malesuada. Quisque pulvinar justo vel rhoncus placerat. Curabitur ultricies facilisis ex semper ultrices. Vivamus vel dui vitae erat iaculis ultricies eu vel augue. Aliquam vel augue interdum, dignissim nisl at, tempor velit.<br></br>
           • Insight 1 description <br></br>
           • Insight 2 Description <br></br>

#### 1.5 PRODUCT COMPARISION

#### 2.3 DATA GATHERING

![google_sheets_tlb](assets/img/portfolio/capstone/google_sheets_tlbs.png)
<br><br>
After creating matching columns in all five tables: ```brand_id```, ```description```, ```colorway```, ```sales```, and ```discount``` in google sheets, I was able to aggregate the data using the ```=QUERY()``` function, however this method achieved only surface level organization. As a result I would need to use bigQuery/SQL to properly clean and analyze the data.
<br><br>
![google_sheets_tlb](assets/img/portfolio/capstone/google_sheets_tlbs.png)

#### 2.4 DATA CLEANING AND MANIPULATION w/ SQL

To prepare the data for analysis, I used BigQuery to join all tables into a single dataset under the ```brand_id``` key. I combined ```product_name``` with ```description``` into one description variable, resulting in a cleaned and aggregated dataset. However, to analyze product category frequencies, I needed to extract string-specific data from the description column. Therefore, I created two variables: ```style_id``` and ```sub_style_id```, and extracted them from ```description``` using ```REGEXP_EXTRACT()``` and Common Table Expressions (CTE) for aggregation. The first variable represented broad apparel categories such as jackets, coats, tracks, vests, etc., while the second variable identified specific properties such as materials or functions (e.g., down, shell, windstop, etc.). See the SQL query below.
<br><br>
```
WITH descrip_tbl AS -- replace decription_id CTE
(SELECT
  brand_id,
  product_name,
  REPLACE(product_name, 'Jkt', 'Jacket') AS description_id,
  
FROM data-analytics-course-413120.gda_course_8_data.outerwear_tbl
GROUP BY brand_id, product_name
),
style_tbl AS  -- Style_id CTE
(SELECT
  brand_id,
  product_name,
    REGEXP_EXTRACT(
    description_id,
    r'Jacket|Vest|Parka|Coat|Fleece|Anorak|Overcoat|
      Peacoat|Gilet|Track Top|Sweatshirt|Shacket|Overshirt'
  ) AS style_id
  FROM descrip_tbl
  GROUP BY brand_id, product_name, description_id
),
sub_style_tbl AS -- Sub_style_id CTE`
(SELECT
  brand_id,
  product_name,
    REGEXP_EXTRACT(
    product_name,
    r'Padded|Down|Linner|Puffer|Bomber|Varsity|Hoodie|Flight|Coach|
    Fleece|Shirt|Track|Packable|Nylon|Chore|Shell|Zip|Ripstop|GORE-TEX|
    Packable|Windrunner|Shearling|Quilted|Wool|Twill|Corduroy|Canvas|
    Flannel|WIP|Denim|Windbreaker|Primaloft®|Cotton|Hoodied|Leather'
  ) AS sub_style_id

FROM data-analytics-course-413120.gda_course_8_data.outerwear_tbl
GROUP BY brand_id, product_name
)
SELECT 
  berlin_ds.brand_id,
  descrip_tbl.description_id,
  style_tbl.style_id,
  sub_style_tbl.sub_style_id,
  berlin_ds.price  
FROM 
  data-analytics-course-413120.gda_course_8_data.outerwear_tbl AS berlin_ds 
  FULL OUTER JOIN descrip_tbl ON berlin_ds.product_name = descrip_tbl.product_name
  FULL OUTER JOIN style_tbl ON descrip_tbl.product_name = style_tbl.product_name
  FULL OUTER JOIN sub_style_tbl ON style_tbl.product_name = sub_style_tbl.product_name

GROUP BY brand_id,description_id, style_id, sub_style_tbl.sub_style_id, price
ORDER BY price DESC
```
#### 2.5 DATA ANALYSIS
![clean](assets/img/portfolio/capstone/cleaned_data.png)
<br><br>
Below is the final dataset after the description variable was aggregated and the string data for ```style_id``` and ```sub_style_id``` were extracted. The dataset is now ready for analysis.
<br><br>

#### 2.6 NULL VALUES
The majority of the nulls values in the dataset were in the ```sale_price``` and ```discount``` columns, both of which were not used in the following report.

#### 2.7 VISUALIZATION
In conclusion, I gathered first-party data from potential competitors to create the dataset, cleaned and manipulated the dataset using Google Sheets and BigQuery/SQL, created two new variables for frequency analysis, and visualized the data using Looker Studio in a comprehensive market report. Below is the completed report with key insights and recommendations. [Click here to view full report](https://lookerstudio.google.com/reporting/b7b2a1d2-341f-4405-9743-5b4390291fa3)
<br><br>
#### 2.8 REPORT PREVIEW
<br><br>
![categories](assets/img/portfolio/capstone/categories_16x9.png)
![down](assets/img/portfolio/capstone/down_16x9.png)
![unique_styles](assets/img/portfolio/capstone/unique_styles_16x9.png)
![canada](assets/img/portfolio/capstone/canada_goose_16x9.png)
![key_takeaways](assets/img/portfolio/capstone/key_takeaways_16x9.png)
