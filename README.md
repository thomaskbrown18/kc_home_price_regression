# Predictions for Housing Prices in King County:

For this project, I explored housing prices in King County Washington and built a model to accurately predict other home prices in the area.  With this model, we can also explore different variables and tell which ones are likely to have the largest impact on price.<br>
The value in this approach is that when investing in real estate or flipping homes for example, you can tell which variables add the most value and focus on those.<br>
This is useful because you can see which homes may be underappreciated, making them a good buy, as well as which areas of the home to upgrade, making them sell for hopefully more than you invested.

## Data Cleaning:

The data for this model was already fairly clean.  It holds 21 variables, and only a few needed some minor cleaning. I completed all the cleaning using Pandas. <br><br>

* **id** - unique identifier for a house
* **dateDate** - house was sold
* **pricePrice** -  is prediction target
* **bedroomsNumber** -  of Bedrooms/House
* **bathroomsNumber** -  of bathrooms/bedrooms
* **sqft_livingsquare** -  footage of the home
* **sqft_lotsquare** -  footage of the lot
* **floorsTotal** -  floors (levels) in house
* **waterfront** - House which has a view to a waterfront
* **view** - Quality of view
* **condition** - How good the condition is ( Overall )
* **grade** - overall grade given to the housing unit, based on King County grading system
* **sqft_above** - square footage of house apart from basement
* **sqft_basement** - square footage of the basement
* **yr_built** - Built Year
* **yr_renovated** - Year when house was renovated
* **zipcode** - zip
* **lat** - Latitude coordinate
* **long** - Longitude coordinate
* **sqft_living15** - The square footage of interior housing living space for the nearest 15 neighbors
* **sqft_lot15** - The square footage of the land lots of the nearest 15 neighbors
<br><br>
After this, I also added data on cities, which I added to reduce the noise in terms of geography.  I grouped over 70 zip codes into roughly 15 cities or regions.  I wanted complete geographic representation in my model, but I did not want to use more than 20 variables for it, so this seemed like a healthy compromise given the size and variety of King County.<br>
# Exploratory Data Analysis
## Impact of Geography on Price:

Which are the top areas in terms of home price?
Here, we'll use a mapping library called Folium to see housing prices in different areas.
This can give us a good idea of where the most expensive areas are:
 
![Imgur](https://i.imgur.com/ZoIKrwz.png)

Grouping by Zip Code gives us a general idea of which zip codes are more expensive than others, but given how large some zip codes are, it makes more sense to use longitude and latitude in the heat map in order to find the higher or lower priced areas.

## Impact of Grade on Price:

From King County Assessment Site: 
- 0-3 Falls short of minimum building standards. Normally cabin or inferior structure.
- 4 Generally older, low quality construction. Does not meet code.
- 5 Low construction costs and workmanship. Small, simple design.
- 6 Lowest grade currently meeting building code. Low quality materials and simple designs.
- 7 Average grade of construction and design. Commonly seen in flats and older subdivisions.
- 8 Just above average in construction and design. Usually better materials in both the exterior and interior finish work.
- 9 Better architectural design with extra interior and exterior design and quality.
- 10 Homes of this quality generally have high quality features. Finish work is better and more design quality is seen in the floor plans. Generally have a larger square footage.
- 11 Custom design and higher quality finish work with added amenities of solid woods, bathroom fixtures and more luxurious options.
- 12 Custom design and excellent builders. All materials are of the highest quality and all conveniences are present.
- 13 Generally custom designed and built. Mansion level. Large amount of highest quality cabinet work, wood trim, marble, entry ways etc.

![Imgur](https://i.imgur.com/odKvlxL.png)

Conclusion: Buying, renovating, and flipping can be highly profitable! Especially if you are taking a 5 rated home (just above average) and upgrading it to a 6 by upgrading the interior and exterior finishing touches. There is a 200K difference between the two medians! It seems very doable to turn a 5 into a 6 with less than 200K, enabling you to capture a profit.

## Impact of Month of Sale on Price: 

Does the month you sell in impact house price?
![Imgur](https://i.imgur.com/VxWLidi.png)
It seems as though month affects average house price very little!<br>
This came as a surprise, but the difference between median house price in December vs April was only ~40K.  While this may be enough to influence some people to wait through the winter, I'm sure we can do better than this.  Let's look at some regression models to see what variables are the most important when it comes to price.  With this, we can determine what would be best to target if you're looking to sell your house.  

# Regression Modeling: 

## Baseline Model:
After some feature engineering and other cleaning steps (also using Pandas), I created this baseline model.  It's important to have a simple model with few transformations or polynomial features in order to have a baseline to compare other models against.

![Imgur](https://i.imgur.com/LTjTwGl.png)

This was a great start!  RMSE is pretty large at around $150K, but r2 is quite high at over .9.  Additionally, there's only a $700 dollar difference between the root mean squared error of the train and test data! This means that so far, we don't have to worry about over-fitting. Right now, our main concern is keeping the r2 high while reducing RMSE. 
<br>Let's now test the following:
- Do log transformed variables improve the r2?
- Would a polynomial regression help?
- Are there any interactions we can use?
- How does including geographic data help?

## Final model:

After testing various models such as polynomial models and a model log transformed variables, I concluded that this final model was the best of the ones I tried.  This model is a multilinear model that uses the city data from 16 different cities/regions referenced before.  This geographic data was one of the main sets of variables that boosted accuracy in terms of r squared and RMSE.  I decided to not use polynomial or log transformed features as it did not improve accuracy but made the model more difficult to explain to a broader audience.<br><br>
Here is a screenshot of the model:

![Imgur](https://i.imgur.com/km9TGLm.png)
<br>
In the end, the model had a R squared value of .948, which means that roughly 95% of the variance in house prices could be explained by this model.  That said, it had a Root Mean Squared Error of roughly 125K.  This means that the average house prediction could vary in accuracy by plus or minus 125K.  While this is high, it likely could not be helped without either eliminating even more outliers, or using much more granular location data.  <br>
I decided to use only 16 cities/regions in order to keep the model clean, and for this simple multi-linear regression model, I stand by that decision even if I could have reduced the RMSE by including 80 plus zip codes as variables.
<br>

Here are some of the more important features from the model:
<br>
Cities:
![Imgur](https://i.imgur.com/5UVO37t.png)<br>
Having a Renovation:<br>
![Imgur](https://i.imgur.com/kozBMjP.png) 

## Further Work To Do:
In the future, I'd love to explore the following features to try to squeeze out a better model:
- Crime data - can we get a better model by including data on crime for each zip code?  High crime in a given area usually correlates with lower priced homes.  I imagine this data could improve the model.
- Distance to important features - Does distance from downtown or the highway boost value?  I've noticed that houses sometimes have "commute scores" or "walkability scores."  I imagine this could improve the model as well.
- Neighborhood data - can we get more granular than zip codes to improve the model?  I would not want to make the model more complicated than it is now, but I'm still curious to see how accurate we can get the model without overfitting.  neighborhood average home prices could help in this regard.

## Conclusion:
Based on my final regression model, we can formulate the following strategy for flipping houses:
- Buy homes in more expensive areas that are undervalued relative to neighbors.
- Renovations help a homes value quite a bit.  All else equal, they can help by around 75,000 dollars.  This is valuable information both for those looking to sell their homes, and people looking to buy and flip homes.
- Use renovations to increase grade and price, and then sell the home, receiving the boost from grade, area, and renovation!
<br>
Using this strategy, one can put in a little work and money to earn outsized returns on what they put in.  It's an older and well known strategy, but using this regression model, you can focus on where the best returns can be found!
<br>
Thanks for reading!
<br>
-Thomas


