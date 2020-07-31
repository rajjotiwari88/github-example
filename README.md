# LOCATION RECOMMENDATION FOR RESTAURANT USING FOURSQUARE API AND MACHINE LEARNING
# 1.Introduction
Choosing a location for your restaurant is one of just a few keys to profitability. Parking and accessibility can be as crucial to a restaurant's success as great food and service. There is a famous saying in real estate that also holds true for restaurants: "You make your money when you buy."

A restaurant's location influences many aspects of your operation, including the menu and style of the dining room. If you already have a certain restaurant location in mind, don’t get too attached until you know if it has all the right requirements for a successful restaurant.

The classic real estate saying “location, location, location” applies to choosing a site for a new restaurant. Location influences the success or failure of a restaurant in a host of ways, from attracting enough initial customer interest to being convenient to visit. But the restaurant’s location is also interrelated to other factors, some of which are changeable, while others are not. A great restaurant location, for instance, must have an affordable rent, or it does not matter how much foot traffic the site receives.

Unfortunately, many restaurants fail within three years, and a lack of planning contributes to this hard reality. Sometimes, restaurants that would be otherwise successful go out of business because they are inaccessible or unknown to their potential customers. As might be expected, visibility and accessibility have a disproportionate effect on new restaurants that are not established yet with a core of loyal customers. Before signing a lease, restaurant owners should not get carried away with any one location until they have done their homework on the location. You must be certain that when you choose a location you are using your head as much as your heart.

We can mitigate this uncertainty through leveraging data gathered from FourSquare's API, specifically, we are able to scrape "likes" data of different restaurants directly from the API as well as their location and category of cuisine. The question we will try to address is, how accurately can we predict the amount of "likes" a new restaurant opening in this region can expect to have based on the type of cuisine it will serve and which city in it will open in. (For the purposes of this analysis, we will contain the geographical scope of analysis to three heavily populated cities in India, namely Delhi, Mumbai, and Bangalore). Leveraging this data will solve the problem as it allows the new business owner (or existing company) to make preemptive business decisions regarding opening the restaurant in terms of whether it is feasible to open one in this region and expect good social media presence, what type of cuisine and which city of three would be the best. This project will analyze and model the data via machine learning through comparing both linear and logistic regressions to see which method will yield better predictive capabilities after training and testing

# 2.DATA
Before we begin analysing the different cities for best location for opening a restraunt, we will first retrieve the geographical coordinates of the three cities (New Delhi, Mumbai and Bangalore). Then, we will use the FourSquare API to obtain URLs that'll fetch raw data in JSON form. We will scrape the raw data separately in these URLs in order to retrieve the following columns: "name", "categories", "latitude", "longitude". and "id" for each city. We can also provide another column ("city") to indicate which city the restaurants are from.

The point to keep in mind here is that the extracts are not of every restaurant in those cities but rather all of the restaurants within a 5KM range of the geographical coordinates that geolocator was able to provide. However, the extraction from the FourSquare API actually obtains venue data so it will include venues other than restaurants such as malls, stores, libraries etc. As such, this means that the data will need to be further cleaned somewhat manually by removing all of the nonrestaurant rows. Once this is complete, we have a shortened by cleaned list to pull "likes" data.The "id" is an important column as it will allow us to further pull the "likes" from the API. We can retreive the "likes" based on the restaurant "id" and then append it to the data frame. Once this is complete, we finally name the dataframe 'raw_dataset' as it is the most complete compiled form before needing any processing for analysis via machine learning.

# 2.1Preparing Data
The data needs further processing before model training and testing.The "categories" column contains too many different types of cuisines to yield any meaningful results.All of the different types of cuisine could be reclassified as European, American, Asian, drinking establishments (bars), or casual establishments such as coffee shops or ice cream parlours. We can implement manual classification as there really aren't that many different types of cuisines. As this project will compare both linear and logistic regression, it makes sense to have "likes" as both a continuous and categorical (but ordinal) variable. In the case of turning into a categorical variable, we can bin the data based on percentiles and classify them into these ordinal percentile categories.As the last stage of data preparation, it is important to note that the regressors are categorical variables (3 different cities and 6 different categories of cuisines). Hence, they require dummy variable encoding for meaningful analysis. We can accomplish this via one-hot encoding.

Did you know? IBM Watson Studio lets you build and deploy an AI solution, using the best of open source and IBM software and giving your team a single environment to work in. Learn more here.
# 3. Methodology
To train and test the data, this project will use linear and logistic regression machine learning methods.Namely, linear regression will be used in an attempt to predict the number of "likes" a new restaurant in this region will have. We will utilize the Sci-Kit Learn Package to run the model. We can also utilize logisitc regression as a classification method rather than direct prediction of the number of likes. Since the number of "likes" can be binned into different categories based on different percentile bins, it is also potentiallly possible to see which range of "likes" a new restaurant in this region will have. Since the "likes" are binned into multiple (more than 2) categories, the type of logistic regression will be multinomial. Additionally, although the ranges are indeed discrete categories, they are also ordinal in nature. Therefore, the logistic regression will need to be specified as being both multinomial and ordinal. This can be done through the Sci-Kit Learn Package as well.

# 4.Results
# 4.1 Linear Regression Results
A linear regression model was trained on a random subsample of 80% of the sample and then tested on the other 20%. To see if this is a reasonable model. the residual sum of squares score and variance score were both calculated (8689 and -0.18 respectively). Given the low variance score, this is probably not a valid/good way of modelling the data. Therefore, we move on to logistic regression.

While analysing the distribution plot of the models we see the distribution of the predicted values is much different from the actual target values. Therefore when testing the model, it doesnt perform upto the mark and hence Linear Regression is not a suitable method to evaluate this model and therefor we move to Logistic Regression.

# 4.2 Logistic Regression Results
A multinomial ordinal logisitc regression model was trained on a random subsample of 80% of the sample and then tested on the other 20%. To see if this is a reasonable model, its jaccard similarity score and log-loss were calculated (0.3673 and 1.48 respectively). Although this is not a perfect prediction, a similarity of 36.73% between the training set and test set is better a result tha linear regression. The classification report is also printed later on below.

Given the modestly accurate ability of this model, we can also run the model on the full dataset. The coefficients show that opening a restaurant in Mumbai or Bangalore, serving cuisine that is american or euro in nature, are associated negatively with "likes."

# 5.Discussion
The first thing to note is that given the data, logistic regression presents a better fit for the data over linear regression. Using logistic regression we were able to obtain a Jaccard Similarity Score of 36.73%, which although not perfect, is more reasonable than the low variance score obtained from the linear regression. As stated before, please note that for the purposes of this project, we are assumming that likes are a good proxy for how well a new restaurant will do in terms of brand, image and by extension how well the restaurant will perform business-wise. Whether or not these assumptions hold up in a real-life scenario is up for discussion, but this project does contain limitations in scope due to the amount of data that can be fetched from the FourSquare API.

As such, to obtain insights into this data, we can proceed with breaking down the results of the logistic regression model. The results showed that the precision score for classifying whether the new restaurant would fall into classes 1, 2, or 3 (lowest, medium, or highest percentile of likes) were 50%, 28%, and 40%. Therefore, the model is better at predicting if a restaurant will fall into the best or worst percentile of likes. This is good as we are mostly concerned with whether the restuarant will perform well or not so the high accuracy of predictions for the two extremum is a welcome feature. This allows us to fairly accurately predict the general performance of the business opportunity. Different binning methods for the classes were attempted, but the use of 3 bins by far yielded the best Jaccard Similarity Score.

Additionally, not only are we attempting to predict the general business performance but also pull insights to inform on business strategy. In this case strategy insight can be gleamed from the coefficient values from running the logistic regressin on the full dataset. As such, we can see that opening a restaurant in Mumbai or Bangalore or serving cuisine that is american or euro in nature, are associated negatively with "likes." This suggests that the business opportunity should be opening a restaurant in Delhi, with a cuisine that is Asian or casual in nature, or serves as a bar would be the best approach for maximizing likes.

# 6.Conclusion
In conclusion, after analyzing restaurant "likes" in India from 300 restaurants, we can conclude that the approach to best take in regards to maximizing business performance (as measured by "likes") is to open a restaurant that is either Asian, or casual or is a bar and that opening the venue in Delhi rather than Mumbai or Bangalore would be the best approach. Additionally, the predictive capabilities of the logistic regression prediction model are most accurate for classifying whether a restaurant will fall in either the best or worst classes when the data is binned into 3 classes.

​