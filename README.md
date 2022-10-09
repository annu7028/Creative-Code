# Creative-Code
ATLS 5660: Creative Code Data Visualization Assignment

In this assignment I explored the price, rating, and diverstiy of different makeup brands with the concentrated focus of their foundations. Rather than an individual chart, I decided to treat this assignment like a data exploration study and therefore created multiple visualizations. 

<b>Notes:</b> 
<ul>
  <li>Not all code is in this ReadMe file. To see full code file please see Python Notebook: [INSERT PATH LINK] </li>
  <li> This file displays all static visualizations. To see interactive visualizations, download and run the python notebook.</li>
</ul>

<b>Questions:</b> 
<ul>
  <li>Do more expensive and luxurious products provide better quality products?</li>
  <li>What is the diversity of 

## Assets
<b>API:</b> https://makeup-api.herokuapp.com/api/v1/products.json?product_type=foundation </br>
<b>Libraries:</b> `pandas`, `altair`, `vega_dataset`

## Dataframe:
Connecting to API, setting up Dataframe, and clean and insert data to only contain relevant data for exploration.

```
file = pd.read_json('https://makeup-api.herokuapp.com/api/v1/products.json?product_type=foundation')
df = pd.DataFrame(file)

df.rename(columns={'id': 'ID', 'brand': 'Brand', 'name':'Name', 'price':'Price', 'rating': 'Rating', 'category': 'Category'}, inplace=True)

df.dropna(subset=['Rating', 'Brand'], inplace=True)
df = df.reset_index()

df = df.drop(columns=['index', 'price_sign', 'currency', 'product_type', 'created_at', 'updated_at', 'product_api_url'])

color_counts = []
#iterate over each row in the dataframe
for index, row in df.iterrows():
    #get the length of product_colors
    color_counts.append(len(row['product_colors']))

df['Color Count'] = color_counts
```

## Price vs. Rating:

<b>Iteration I:</b> Explore Price vs. Rating of individual products through a scatterplot. After analysis found that representation in this way was inconclusive and confusing.

![Viz 1](https://github.com/annu7028/Creative-Code/blob/annu7028-dataVizAssignment/visualization.png?raw=true)

<b>Iteration II:</b> In an effort to make the graph more readable, I explored the use of binned scatterplots finding that price did not necessarily equate to quality of product.

```
alt.Chart(df).mark_circle(size=200).encode(
    alt.X('Price', bin=True),
    alt.Y('Rating'),
    color='Brand',
    tooltip=['Brand', 'Name', 'Price', 'Rating', 'product_link']
).configure_axis(
    grid=False
).interactive()
```
![Viz 1](https://github.com/annu7028/Creative-Code/blob/annu7028-dataVizAssignment/visualization-2.png?raw=true)

## Brand Prices and Quality:
After coming to the above conclusion, I decided to explore Brand Prices and Quality of those brands.

<b>Iteration I:</b> In iteration I explored the average price of each brand. I found that in terms of price the most expensive brands were as follows:

<ol>
    <li>Dr. Hauschka</li>
    <li>Mineral Fusion</li>
    <li>Cargo Cosmetics</li>
</ol>

![Viz 1](https://github.com/annu7028/Creative-Code/blob/annu7028-dataVizAssignment/visualization-3.png?raw=true)

<b>Iteration II</b> explores brands and their overall ratings in terms of Foudnation. Here I found that average ratings were never below an average of 3.5. My conclusion of this iteration found that the best average ratings belonged to the brands `Annabelle`, `Cargo Cosmetics`, and `Marcelle`.

![Viz 1](https://github.com/annu7028/Creative-Code/blob/annu7028-dataVizAssignment/visualization-4.png?raw=true)

<b>Iteration III</b> explored the combination of average price, average rating, and brand. The bar chart displays the aveage price of the different brands with the darkest bars in the color gradient being the best rated.

```
alt.Chart(df).mark_bar().encode(
    alt.X("Brand", axis=alt.Axis(title='Brand')),
    alt.Y("mean(Price):Q", axis=alt.Axis(title='Average Price')),
    color = alt.Color("mean(Rating)", scale=alt.Scale(scheme="goldorange"))
)
```
![Viz 1](https://github.com/annu7028/Creative-Code/blob/annu7028-dataVizAssignment/visualization-5.png?raw=true)

## Color Diversity:
While it appears clear what brands would be considered the "best bang for your buck" it fails to consider the constant discrimination that occurs within the makeup industry. For this reason, it is important to look at the color diversity of brands and products. 

<b>Iteration I</b> explores the cost of products in relation to the amout of colors provided for that individual product. As shown below it is found that price and color diversity have little to no relation with most products.

![Viz 1](https://github.com/annu7028/Creative-Code/blob/annu7028-dataVizAssignment/visualization-6.png?raw=true)
![Viz 1](https://github.com/annu7028/Creative-Code/blob/annu7028-dataVizAssignment/visualization-7.png?raw=true)

<b>Iteration II: Brands and Color Diversity</b> explores how brands fair with color diversity. Finding that brands such as `Pure Anada`, `Maybeline`, and `L'Oreal` have the best color diversity.

![Viz 1](https://github.com/annu7028/Creative-Code/blob/annu7028-dataVizAssignment/visualization-8.png?raw=true)

<b>Iteration III: Cost and Diversity</b> 
From this, I wanted to add the extra layer of looking at the price. Seeing if cheaper or more expensive brands provided better color diversity. From the graph below I display each brands, color diversity with the darker bars showing higher prices and lighter bars showing lower prices.
From this we once again see that price doesn't necessarily equate to color diversity. It does however, give us insight into the ethicacy and market that quite a lot of brands are missing out on.

```
alt.Chart(df).mark_bar().encode(
    alt.X("Brand"),
    alt.Y("mean(Color Count)", axis=alt.Axis(title='Color Diversity')),
    color = alt.Color("mean(Price)", scale=alt.Scale(scheme="yellowgreenblue"))
)
```
![Viz 1](https://github.com/annu7028/Creative-Code/blob/annu7028-dataVizAssignment/visualization-9.png?raw=true)

<b>Iteration IV: Rating and Diversity</b>
I was then curious to see how brand ratings compared to their color diversity. Are these more diverser products providing quality? The graph below once again displays Brand vs. Color Diversity with higher rated products being dark blue, and lower rated products being light green.
I found it once again interesting that brand average rating and color diversity didn't have too great of a relation.

```
alt.Chart(df).mark_bar().encode(
    alt.X("Brand"),
    alt.Y("mean(Color Count)", axis=alt.Axis(title='Color Diversity')),
    color = alt.Color("mean(Rating)", scale=alt.Scale(scheme="goldorange"))
)
```

![Viz 1](https://github.com/annu7028/Creative-Code/blob/annu7028-dataVizAssignment/visualization-10.png?raw=true)

<i>Additional vizualizations that explore this idea:</i>
![Viz 1](https://github.com/annu7028/Creative-Code/blob/annu7028-dataVizAssignment/visualization-11.png?raw=true)
![Viz 1](https://github.com/annu7028/Creative-Code/blob/annu7028-dataVizAssignment/visualization-12.png?raw=true)

# Conclusion:
Price does not equate to quality of product

Most expensive brands:
<ol>
    <li>Dr. Hauschka</li>
    <li>Mineral Fusion</li>
    <li>Cargo Cosmetics</li>
</ol>

Best rated brands:
<ol>
    <li>Annabelle</li>
    <li>Cargo Cosmetics</li>
    <li>Marcelle</li>
</ol>

While products like <b>Mineral Fusion</b> are not worth the expensive price products from <b>Dr. Hauschka</b> might be worth the extra money spent. However, you can get better products for a lower price from <b>Cargo Cosmetics</b>, <b>Marcelle</b>, and <b>Annabelle</b>. The best brand for the lowest price was overwhelmingly `Annabelle`

Overwhelmingly, color diversity is not considered into account in terms of cost or ratings of products.
Brands such as `Pure Anada`, `Maybeline`, and `L'Oreal` have the greatest color diversity.
