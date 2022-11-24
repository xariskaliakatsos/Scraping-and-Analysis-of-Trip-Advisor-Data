# Scraping-and-Analysis-of-Trip-Advisor-Data
This project regards extraction of textual data from TripAdvisor reviews and their preprocessing and analysis in order to find out useful insights. More specifically, the reviews in TripAdvisor for restaurants in Thessaloniki are scraped using Selenium. Also, they are analyzed using Python in order to create interesting visualizations and a sentiment analysis is conducted to extract the sentiment of the reviews. Finally, a classifier is trained to predict the gender and the age group of the reviewer based on the text of the review.

## Table of Contents

[0. Installation](https://github.com/xariskaliakatsos/Scraping-and-Analysis-of-Trip-Advisor-Data#0-installation)

[1. About](https://github.com/xariskaliakatsos/Scraping-and-Analysis-of-Trip-Advisor-Data#1-about)

[2. Web Scraping Process](https://github.com/xariskaliakatsos/Scraping-and-Analysis-of-Trip-Advisor-Data#2-web-scraping-process)

[3. Data Analysis](https://github.com/xariskaliakatsos/Scraping-and-Analysis-of-Trip-Advisor-Data#3-data-analysis)

[4. Sentiment Analysis](https://github.com/xariskaliakatsos/Scraping-and-Analysis-of-Trip-Advisor-Data#4-sentiment-analysis)

[5. User Profiling](https://github.com/xariskaliakatsos/Scraping-and-Analysis-of-Trip-Advisor-Data#5-user-profiling)


## 0. Installation 

The code requires Python and Selenium.

## 1. About

**Analysis of TripAdvisor Data** is a project that was created as a semester Project in the context of “Web Mining” class. MSc Data and Web Science, School of Informatics, Aristotle University of Thessaloniki. The project team conisted of me, Vasiliki Parousidou, Enangelos Bektis, Georgios Aswestopoulos.

## 2. Web Scraping Process

The information that we want to scrape from each restaurant review using Selenium with chromedriver is the following:
- Username of the reviewer
- Age of the reviewer
- Gender of the reviewer
- Location of the reviewer
- Review distribution of the reviewer
- Name of the business
- Review date
- Visit date of the reviewer
- Title of the review
- Text of the review
- Rating of the review

![image](https://user-images.githubusercontent.com/95586847/180650940-99ea59bc-c78c-481f-aed4-f7e588224148.png)

The process followed to scrape the data is described below:
- Find the way TripAdvisor is set up using F12
- Collect the restaurant links
- Collect the important elements of each review 

  Review             |  Reviewer
  :-------------------------:|:-------------------------:
  ![image](https://user-images.githubusercontent.com/95586847/180652283-ba69e0e6-3c01-49c2-81db-d2d46033b270.png)  |  ![image](https://user-images.githubusercontent.com/95586847/180652293-143ed578-2415-47ed-8fff-808f309675f5.png)

The whole scraping process took around 15 hours for 14373 reviews.

## 3. Data Analysis

### 3.1 Preprocessing 
- Drop unwanted columns
- Process "Review" column
  - Remove punctuation marks
  - Lowercase
  - Tokenize (split each review into words)
  - Remove stopwords
  - Stemming (produce morphological variants of a root word)
- Process "Rating_Date" column
  - Extract review's month 
  - Extract review's year

Final format:
![image](https://user-images.githubusercontent.com/95586847/180653391-b4747f1d-81be-4308-8d02-c5ac59e003fc.png)

### 3.2 Monthly Reviews
Visualize the monthly number of reviews:

<p align="center"><img src="https://user-images.githubusercontent.com/95586847/180653719-ea37a8a2-03c2-48cf-852a-a72c1dd39e2a.png" width="500"></p>

The month with the most reviews made is August with September trailing right behind. This is due to the tourism Thessaloniki gets late at summer. Despite the peak observed during summer, September and October, we can see that the number of monthly reviews remains the same during the rest of the months.

### 3.3 Most Common Words, Bi-grams, Tri-grams
The library "WordCloud" was used to visualize unigrams, bigrams and trigrams that occur frequently at 1-star and 5-star reviews.
- Unigrams
  1-star             |  5-star
  :-------------------------:|:-------------------------:
  ![image](https://user-images.githubusercontent.com/95586847/180654770-2e3d2e93-3cbd-434a-9526-45448c80112b.png) |  ![image](https://user-images.githubusercontent.com/95586847/180654828-091083c3-afd0-4b14-a175-e4198324fbbc.png)

- Bigrams
  1-star             |  5-star
  :-------------------------:|:-------------------------:
  ![image](https://user-images.githubusercontent.com/95586847/180654787-87f3e4c9-e06d-46c0-b405-0371010302d0.png)  |  ![image](https://user-images.githubusercontent.com/95586847/180654838-f1e737cc-96cf-4078-bcb9-74ac53c166b2.png)


- Trigrams
  1-star             |  5-star
  :-------------------------:|:-------------------------:
  ![image](https://user-images.githubusercontent.com/95586847/180654801-6c14bc94-01d9-44cf-aa1a-0da743b3d19f.png) |  ![image](https://user-images.githubusercontent.com/95586847/180654854-d6f4367a-6ed4-412d-985e-fb241caa2a47.png)

It is obvious that the bigger the "n" gets, the better it describes the general feeling of a review. Unigrams are just common words, while some of the bigrams and most of the trigrams give a more complete view of the context. 

### 3.4 Fastest Growing and Fastest Shrinking Words
Another interesting visualization would be to explore the fastest growing and shrinking words across the time.
For instance:
"Vegan"            |  "Soutzoukakia"
:-------------------------:|:-------------------------:
![image](https://user-images.githubusercontent.com/95586847/180656507-006479a8-d13c-4b06-ae92-47894bdcbbf7.png) |  ![image](https://user-images.githubusercontent.com/95586847/180656524-9da3fc25-21f2-4542-bbf2-b7a4670cc424.png)


### 3.5 Topic Modeling 
We try to extract and visualize emerging topics from all the reviews across time using Latent Dirichlet Allocation (LDA) algorithm. We split the reviews in chronological order and find the four most representative topics.
<p align="center"><img src="https://user-images.githubusercontent.com/95586847/180656871-24bb1cd1-c18c-4c55-9c6d-58150495b3d8.png" width="500" height="500"></p>

### 3.6 Map of Locations
We also visualize in a map the location of reviewers so that we can make insightful conclusions about the tourism in Thessaloniki.
- Get the country given the city
- Get the coordinates
- Create the map

<p align="center"><img src="https://user-images.githubusercontent.com/95586847/180658666-73d14e7c-ff09-41d5-8ff9-a389f72c5baa.png" width="600" height="400"></p>

## 4. Sentiment Analysis 
We also extract the sentiment of the reviews using the "TextBlob" library.

### 4.1 Polarity Score
TextBlob library was used to get the sentiment polarity score of the reviews and find those with the highest and lowest sentiment polarity scores. The polarity score is a float within the range [-1.0, 1.0], where values larger than zero mean positive sentiment. The top-5 positive and top-5 negative reviews are presented below:
"Positive"            |  "Negative"
:-------------------------:|:-------------------------:
![image](https://user-images.githubusercontent.com/95586847/181086538-f1bfb344-51f5-43a2-96b4-1c2255988cc0.png) |  ![image](https://user-images.githubusercontent.com/95586847/181086421-b0ae3e4c-a915-4b1c-a67d-af15dfebd3fb.png)


### 4.2 Polarity Score per Restaurant 
We also calculate the polarity score for each restaurant based on its reviews in order to find the top-10 restaurants with positive and negative scores. Since these mean polarity scores may be misleading if a restaurant has very few reviews, we take into account only the restaurants that have more than 20 reviews.

"Best Restaurants"            |  "Worst Restaurants"
:-------------------------:|:-------------------------:
![image](https://user-images.githubusercontent.com/95586847/183750652-a1476dd1-9473-4da4-ac78-aa962668441f.png) |  ![image](https://user-images.githubusercontent.com/95586847/183750670-74a331a1-fa26-4181-8612-a2bd0a7c5ab9.png)

## 5. User Profiling
Next, we build Machine Learning models that predict the gender and the age group of the reviewer based on the review. More specifically, the features that will be used are the review text, the user review distribution, the rating and the polarity that was calculated in the previous section.

### 5.1 Gender
The percentage of women and men for the reviewers that we have this information in the dataset are presented in the following pie chart. We have information about the reviewer's gender in about one third of our dataset.

<p align="center"><img src="https://user-images.githubusercontent.com/95586847/183751581-6b484d57-88a5-42f9-b658-c648cff18d2e.png" width="300"></p>

We split our dataset into training and test set and we use a "Single Imputer" so as to complete the missing values using the mean along each column and a “ColumnTransformer” in order to convert each review to a vector of TF-IDF features. After that, in order to select the most appropriate classification algorithm, we further split the training data into training and validation sets. The results are presented below:

<p align="center"><img src="https://user-images.githubusercontent.com/95586847/183754910-004f3aaa-31db-4179-a7dd-fdbda8757dff.png" width="350"></p>

Since the largest accuracy (0.70) is achieved using the Random Forest classifier, this classifier is selected in order to predict the gender of the reviewers that we do not know their gender. The percentage of women and men in the whole dataset are presented in the pie chart below.

<p align="center"><img src="https://user-images.githubusercontent.com/95586847/183755367-cd894bdf-20f0-4b4c-936b-0b3340b1449e.png" width="300"></p>

We also present the most used words by men and women in the following wordclouds.
"Men"            |  "Women"
:-------------------------:|:-------------------------:
![image](https://user-images.githubusercontent.com/95586847/183844705-b3143773-4a4d-465e-be56-600a07f487e9.png)  |  ![image](https://user-images.githubusercontent.com/95586847/183844738-8a548306-e152-4b31-9e5e-1e6fa5d6a173.png)

### 5.2 Age Group
The percentage of each age group for the reviewers that we have this information in the dataset are presented in the following pie chart. We have information about the reviewer's age in about one third of our dataset.

<p align="center"><img src="https://user-images.githubusercontent.com/95586847/183845506-ac70b1fa-b435-4f57-ba7c-f6a3df9ab449.png" width="300"></p>

The features that are used to create our model are the same as before. The results of the three classification algorithms that were tested are presented below.

<p align="center"><img src="https://user-images.githubusercontent.com/95586847/183846101-d7c5f0ad-bd6d-4293-b27e-1d672c606ec5.png" width="350"></p>

In this case “Logistic Regression” yields the best results, although the accuracy is not large enough (0.47), yet larger than the accuracy of the random classifier (0.2). Thus, “Logistic Regression” is selected in order to predict the age group of the whole dataset. The percentage of each age group in the whole dataset is presented in the pie chart below:

<p align="center"><img src="https://user-images.githubusercontent.com/95586847/183846314-3411e1c8-a326-4910-b863-c7e657acd131.png" width="300"></p>

Finally, we create the boxplots of polarities of each age group.

<p align="center"><img src="https://user-images.githubusercontent.com/95586847/183847300-73ddaf09-0afa-4319-ba10-288208c4ebdb.png" width="400"></p>


