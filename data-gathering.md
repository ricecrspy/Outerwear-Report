# BERLIN DATA GATHERING

## DATA MINING
I identified five e-commerce boutique stores that align with Berlin's market positioning in terms of price, product design, target customer, and quality. Using a Chrome browser web crawler extension, I scraped over 300 rows of data observations, about 30-70 rows from each website, based on the following variables: brand name, product description, colorway, price, discount, and sale price. Data structures varied from store to store, and as a result, a unique table was created for each, totaling five tables. <br></br>

           • 300 rows of data observations (30-70 rows from each website) <br></br>
           • Following variables: brand name, product description, colorway, price, discount, and sale price <br></br>

[IMAGE]<br></br>

## DATA INTERGRATION
After creating matching columns in all five tables: brand_id, description, colorway, sales, and discount in google sheets, I was able to aggregate the data using the =QUERY() function, however this method achieved only surface level organization. As a result I would need to use bigQuery/SQL to properly clean and analyze the data

[IMAGE]<br></br>
