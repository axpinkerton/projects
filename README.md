# projects
Inspired by the rapid acceleration in the shift from traditional brick and mortar to eCommerce retail transactions set forth by the Covid-19 pandemic, I wanted to explore the transaction-level data of an eCommerce retailer to pull out useful insights into the consumer behavior & what it suggests about the business at large
I had several main questions going into my analysis:
1. What can I infer about the high-level seasonality of the retailer?
2. What do the product lifecycles look like? 
3. How does markdown and promotional pricing affect consumer behavior?
4. What insight can I glean about product times on offer and launches of new product?
5. Is there a ‘drop-off’ at a certain point in a product’s lifetime? If so, can wasteful inventory cost be eliminated by buying to sell through by the start of the drop-off?
6. Is a significantly disproportionate percent of the business coming from the top products? In theory, there isn’t as much of a cost tradeoff online--one additional product online doesn’t have the same ‘shelf space’ tradeoff as brick and mortar

The data that I am working with came from Kaggle and is comprised of 541,909 observations of the following attributes & metrics: Invoice Number, Product Stock Code, Product Description, Product Quantity in order (Aggregated by product, e.g. 6 units of the same product in one order will show in one row, not 6), TimeStamp of Order, Product Unit Price, Customer ID, and Shipping Country
Data covers the timespan from late 2010 through late 2011

Kaggle Link:
https://www.kaggle.com/vijayuv/onlineretail?select=OnlineRetail.csv

The first thing I did was to break down and covert the Order timestamp to a standard more usable for aggregating--I used standard calendar months for month aggregation, and the National Retail Federation’s 445 week numbering system (starting the year in January) to get week attributes
All aggregations at either the total year, calendar month, or 445 week level
Then, multiplying out Unit Price and Unit Quantity, I got the revenue (in Great British Pounds) associated with the Product-Order, and creating a Boolean Flag representing if the transaction was a Return (negative sales quantity) or a Sale (positive sales quantity)
After establishing the monthly business trend, I can dig into the details of monthly and weekly product performance and discover possible reasons for the data actualizing in that manner
For reference, UK represented 91% of transactions, transactions in the EU were around 98%

Data Exploration
First, I wanted the Net Sales by calendar month--best to see high-level variations and trends in the business
As you can see in the Net Sales by month graph, sales are quite concentrated in the back half of the year--Not terribly unusual for any retailer, but a bit more pronounced here than the standard (Standard is 40-45/55-60 Front Half/Back half split, here is 37/63)
The delta between each month’s Revenue number and the Unit sales is the AUR or avg. unit retail price by month--again this is highest in Nov
The peak sales month is November, with a dip going into December and a huge debuild from Dec to Jan, which again is pretty standard, but the drop between December and January is really significant
Going solely off of the product descriptions, the assortment does seem to be focused around gifting and party-related products, like decorations so that likely explains the spike around the holidays
Can we dig into the debuild from Dec to Jan and learn anything? Is timing of when returns are actualizing a factor?

Gross sales & Returns
Looking at gross sales and returns separately is a better representation of the business than net sales alone

I wanted to look at the Gross Sales and returns separately to get a sense of how long after sales occur do return transactions get processed into the system--returns actually spike in December (from Nov sales) and the Jan returns are certainly higher than the yearly avg., but have a more pronounced effect due to the drop in gross sales
Didn’t expect to see sales drop from Jan to Feb--likely attributed to New Years’ promotions and sales (Can see the relatively low AUR in Jan)
Gross sales in Dec barely debuild from Nov, but the net effect of returns from Nov hurt Dec net sales

Return Rate by Month (as a % to Ttl Sales)
Spikes in the months that typically have promotions--Nov & Dec holiday sales and Semi-Annual/End of Season Sales in June
Easy to see from this graph just how significant an impact the returns from Sales months Nov & Dec have on Dec & Jan’s net revenue numbers

Regular Price and Markdown Product Performance
Next I wanted to get a better sense of the complexion of the products that made up the sales--namely, how much was being driven from regular versus markdown or promotional pricing
In order to arrive at the final result of a transaction-level product markdown flag, I isolated the Stock Code and Unit Price, removed duplicates and then sorted by stock code and then unit price, highest to lowest. If a certain stock code only had one unit price associated with it, that price was assumed to be the Full/Regular Price. For stock codes with multiple unit prices across transactions, I treated the highest unit price as the Full/Regular Price, and each of the lower prices as Markdown or Promo prices. From this I also noted if a product was ever marked down or promo’d in the year or not, for use in a later analysis. 
Due to not actually having the product’s original retail price and needing to back into the Reg/Markdown flag, I am likely overestimating the Regular Price transactions--despite this, my calculated Regular Price sales are only 6% on the year
This likely comes from the retailer training their customer to not buy product at full price--If customers know that they can eventually get a product at a lower price from the retailer, or if the retailer tends to have some promotion running, there is little incentive to buy at full price. This is a dangerous cycle for the long-term health and sustainability of a retailer

Top 25 Line Items (by Net Rev.) Weekly Performance
Next I looked at weekly performance for the Top 25 products at the weekly level
Cyclical peaks and valleys emerged for most of the products month to month, indicating a monthly traffic driver for customers to the site of some sort--possibly a catalog, or email blast, with the highest volume spike in holiday months
Top 25 list was compiled at the year level--there are a few non-product items, like shipping, included in the top revenue-driving line items
Two interesting takeaways from this view--first the fairly clear monthly cycle for most of the products in the top 25. Something is causing a large increase in traffic and sales to the site every month, causing a huge increase in sales during one week of the month, and then very quickly sales decrease to their previous normal weekly pattern. This same cycle does not hold for the last two months of the year, however.
My second surprising takeaway from this graph was the fairly ‘evergreen’ nature of the assortment--this could be because I am only looking at the top 25 products on the year level, but I expected there to be seasonal products that had one or two big months of sales and then drop off completely, but from this view that seems to not be the case. The Assortment seems to be fairly consistent throughout the year, and drive fairly consistent sales
Top 25 represents 16% of total revenue on the year, and 8% of sales units (.6% of Stock Codes)

Scatter Plot of Sales for all Products
Looking at the Sales Units on one axis and sales revenue on the total year for every product offered unsurprisingly shows a cluster of products grouped in the first quadrant--outliers on this graph are often non-inventory items & fees
I’d love to get more data for these products to do a proper SKU rationalization, grouping into Low and High Volume Sale unit products, and Low and High volume margin products to get a sense of what sort of products and categories can be culled from the assortment

Product Lifecycle
Charting the lifecycle of all products, I expected there to be a much shorter average timeline for products. 8 to 12 weeks is fairly standard, followed by a steep decline where remaining inventory was marked down and slowly sold off. This retailer’s assortment is proving to be very steady--there is a spike after the product introduction, and sales stay steady until around week 31
To find the performance of a product over the course of its product lifecycle, I found the first week that a stock code showed up in transactions--I ignored the first 8 weeks of transactions, assuming that I couldn’t count anything as being launched in the first 8 weeks of transactions since there was no way to differentiate between something truly being launched and product that had been previously launched but I simply didn’t have the transactions yet. January product launches are also pretty small when comparing to the rest of the year, so I figured not much was lost.
The drop off that is seen in week 31 could very well be seasonality driven, and I believe that to be the case--I would like to have more data to either confirm or reject that hypothesis

Results & Conclusion
Finishing this analysis, I have a much better understanding of the eCommerce retailer’s business:
Sales are heavily concentrated in the back half of the year, with demand peaking in November, and high returns in both December and January
Customers are quite price-resistant, with only 6% of yearly sales coming from regular price product
Product lifecycle is quite consistent from launch, and based on the transaction data that I analyzed, sales do not drop off until around week 31 after the product launch--I would like to dig into this further with transaction data across multiple years to confirm, as the drop off could be seasonality-driven
Comparing all products sales performances, there doesn’t seem to be a trend as far as what products could be eliminated without major effect on the business, but a deeper analysis on that subject would require Inventory and Buy data, as well as Categorical information for the products
Since the lifecycle is so consistent and flat week to week, inventory liability should not be much of an issue for the retailer, as most products tend to drive consistent sales regardless of the month of the year


Contact
Alex Pinkerton


www.linkedin.com/in/alexanderpinkerton


