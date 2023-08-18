# ChurnPrediction

Sparkify is a music streaming service ( like Spotify, Pandora, etc. ). In Sparkify users can either listen to music for free or buy a subscription. The free users will listen to ads while the subscription users ( or paid users ) will listen to songs ad-free. Users need a login to listen to a song on the service.
From a subscription perspective at any point, users can do any of the following: 
1. Upgrade from the free tier to the paid tier
2. Downgrade from the paid tier to the free tier
3. Cancel their account and leave the service
We want to identify the users that could potentially cancel their account and leave the service. By identifying this population upfront, we could incentivize them by giving them discounts or other rewards. This could make them stick, giving us a loyal customer base which is key for a company’s growth.

<img width="695" alt="image" src="https://user-images.githubusercontent.com/58876667/212476880-630b6f8e-78e9-4942-8785-1362912b6fef.png">

We have built an end to end data pipeline starting with data streaming from a log file emulating the application backend to the Kafka cluster. There will be a dedicated Spark streaming application to consume the stream message and ingest to local file storage emulating Hadoop, stored as json files, and retrieved with Spark Dataframe API via Jupyter to perform Churn Prediction analysis. The experiment focuses on analyzing and predicting the factors impacting churn rate.

## Kafka Streaming

The kafka producer server parses the json log file and prepares the message to be sent over as a stream of data. The app.cfg lists out parameters used in the application (specific for Kafka and Spark). The kafka producer notebook contains the logic to send the kafka message. A sample kafka consumer notebook is used for testing the message sent by the kafka producer. In the Spark streaming notebook,weconsumethe messagefromKafka,transformandstoreonlocalfile storage.

## Dataset(Schema)

<img width="474" alt="image" src="https://github.com/AAKASHKSHETTY/Big_Data_Music_Churn_Prediction/assets/58876667/6215f5f1-1d42-430a-ad5d-8cb355551e0d">

## Defining churn

Churn refers to the action of canceling a user's account on the platforms. Another term for a churned user is one that has canceled their account and left the platform.

## Data Visualization

Out of all events, we pay specific attention in Cancellation Confirmation, it's an event that we never like to see a user doing.We use this event to flag and create a churn column.

The data divide will check the user churn ratio, to define how many users have actually churned and how many users are still using the music app.

Churned: 22.46%

### Gender Count

The dataset contains both male and female users and thus presents a ratio divide in our dataset.

<img width="461" alt="image" src="https://github.com/AAKASHKSHETTY/Big_Data_Music_Churn_Prediction/assets/58876667/dc4db8bf-2a63-487a-a881-04ff2ff44a5c">

Thus, we can see there are more male users than female users in our dataset.

### Unique user sessions (Length)

The dataset contains values in reference to a huge number of sessions, but also includes the values of each page visit for each user. Thus, using the PySpark Query, we count the number of distinct sessions length for each user. In other words, sorting the highest amount of time a user spent on the app in a single session.

<img width="471" alt="image" src="https://github.com/AAKASHKSHETTY/Big_Data_Music_Churn_Prediction/assets/58876667/27976d04-8433-49a7-b3d9-b6be2fb8cc4b">

Length looks like a gaussian distribution with a long write tail. Most length values are concentrated between 0 and 500.

### User Level (Free Vs Paid)
The user on the app is either a paid subscriber or a free user and thus would be helpful to determine if it is the reason for the user to churn. We would explore the distribution of the free and paid subscribers in our event log distribution.

<img width="197" alt="image" src="https://github.com/AAKASHKSHETTY/Big_Data_Music_Churn_Prediction/assets/58876667/bdfa4ff9-40de-4918-862a-19569095541a">

Also, the user interaction was found to be the value of 12700 which signifies the amount of time these free users on the app integrated with the paid subscription users.

### Local Analysis (State)
 
It is very important to determine where the users on our app are present and situated. This would help the marketing team to better understand how the user base in those areas can be grown either by applying discounts or giving free trials.

<img width="716" alt="image" src="https://github.com/AAKASHKSHETTY/Big_Data_Music_Churn_Prediction/assets/58876667/57a37848-45bc-4d50-a0ac-a3101962c661">

California has the widest user base, followed by texas and new york state.

### Page Count Analysis

The number of user counts that visited different pages or how they interacted with the app, most likely the page events they visited.

<img width="402" alt="image" src="https://github.com/AAKASHKSHETTY/Big_Data_Music_Churn_Prediction/assets/58876667/3dfef417-0319-4386-9a2e-096295cc38cd">

Most of the users are in NextSong, Home, Thumbs Up or Add to Playlist When we have few that cancel, or in Cancellation Confirmation or Submit Downgrade.

### User Analysis (Day & Time)

Below depicted charts help users to understand the user timestamp behavior on the platform. It helps us to track the highest period of activity on the user app. At what day or time, does the user use the music app the most.

<img width="571" alt="image" src="https://github.com/AAKASHKSHETTY/Big_Data_Music_Churn_Prediction/assets/58876667/65b55407-1ec7-4419-9e00-03468e05e099">

<img width="583" alt="image" src="https://github.com/AAKASHKSHETTY/Big_Data_Music_Churn_Prediction/assets/58876667/46b7f001-2a60-4d82-9689-a14435fa4d0f">

<img width="583" alt="image" src="https://github.com/AAKASHKSHETTY/Big_Data_Music_Churn_Prediction/assets/58876667/b20aca37-11f1-40b2-94b0-7e3d5c7fb573">

Users' behaviors are periodic, they use Sparkify more often on weekdays than weekends. In one single day, they use Sparkify more often after 14 o'clock.

### Effect of each feature on churn (Analysis)

Let’s analyze how these features had the effect on the churn. The below references present how each feature in our event dataset was used to affect the user churn like the questions were answered using Pyspark queries whether the male users churned more in reference to the other female users or does the user session length had any effect on the churn prediction.

<img width="916" alt="image" src="https://github.com/AAKASHKSHETTY/Big_Data_Music_Churn_Prediction/assets/58876667/2851307e-41ae-49ca-932a-ee320da5b62c">

### Location (Churn Effect)

<img width="490" alt="image" src="https://github.com/AAKASHKSHETTY/Big_Data_Music_Churn_Prediction/assets/58876667/cecc8ff4-2898-4d05-92d7-d8a96129504c">

### Page (Churn Effect)

<img width="811" alt="image" src="https://github.com/AAKASHKSHETTY/Big_Data_Music_Churn_Prediction/assets/58876667/2c812251-b30a-467c-8982-67c7c90c28a6">

NextSong,Thumbs Up/Down, Home, Add to Playlist page's seems to have an effect on Churn or not.

## Feature Engineering

Feature engineering is the process of using domain knowledge of the data to create features that make machine learning algorithms work. If feature engineering is done correctly, it increases the predictive power of machine learning algorithms by creating features from raw data that help facilitate the machine learning process. Feature Engineering is an art. Some of the ideas we have used are mentioned below about how the original set of features were changed to derive a new set of features that were more relevant in predicting the user churn.

In this part, we create other features from the existing ones. For example, the number of times that the user has an event on the page Submit Downgrade.

becomes the number of downgrades. Some features are ignored like the firstName and lastName because they appear to be irrelevant by the aspect of churn. Others, like the location and the userAgent, are complex to extract something important.

### User Session (Count of each page visit as feature set for new data)

Using the pyspark order by feature to create new column features based on the actions users have taken in the event logs.

<img width="958" alt="image" src="https://github.com/AAKASHKSHETTY/Big_Data_Music_Churn_Prediction/assets/58876667/f5c54fb7-a2a3-410e-87a5-5491cf4e907b">

Adding the average playback time as a feature to check how users have played songs per session

### User Profile

Creating user dimension features for subscription, streaming, community and page visit counts based on user interaction

### User Dimension (Daily Data) - Adding daily

<img width="959" alt="image" src="https://github.com/AAKASHKSHETTY/Big_Data_Music_Churn_Prediction/assets/58876667/631440f3-e506-42fe-a506-1fd4702ab7a9">

### The final features

● male:1—male or 0—female
● paid: 1- paid subscription or 0 — free subscription
● avg_daily_actions: Average actions by day
● avg_session_duration: Average duration in hours of each session
● avg_playback_time: Average time listening to music for each session
● n_actions: The number of actions
● n_added_to_playlist: Number of songs added to the playlist
● n_ads: Number of ads viewed
● n_days: Number of days of the observed window,
● n_dislikes: Number of dislikes
● n_downgrades: Number of downgrades
● n_errors: Number of experienced errors
● n_friends: Number of friends added
● n_help: Number of times has accessed the help page
● n_home: Number of times has accessed the home page
● n_likes: Number of songs liked
● n_sess: Number of sessions
● n_settings: Number of times has accessed the help page
● n_songs: Number of songs played
● n_upgrades: Number of upgrades
● n_ads_over_songs: Number of ads viewed divided by Number of
songs played
● n_likes_over_songs: Number of songs liked divided by the number of
Songs
● n_dislikes_over_songs: Number of songs disliked divided by the
number of Songs
● n_likes_over_dislikes: Number of songs liked divided by the number
of songs disliked
● time_window: Time time in hours of observed data

## Data Modeling

### Gradient Boost and Random Forest

The idea of boosting came out of the idea of whether a weak learner can be modified to become better. A weak hypothesis or weak learner is defined as one whose performance is at least slightly better than random chance. Gradient Boost Trees is the modified and improved version of AdaBoost.

AdaBoost is an algorithm for constructing a “strong” classifier as a linear combination of “simple” “weak” classifiers. In other words, the main working principle is to convert a set of weak classifiers into a strong one. Weak classifier is described as less than 50% error over any distribution and strong classifier is thresholded linear combination of the weak classifier outputs.

Gradient boosting involves three elements:

● A loss function to be optimized. The loss function used depends on the type of problem being solved. It must be differentiable, but many standard loss functions are supported and you can define your own. For example, regression may use a squared error and classification may use logarithmic loss
● A weak learner to make predictions. Decision trees are used as the weak learner in gradient boosting
● An additive model to add weak learners to minimize the loss function. Trees are added one at a time, and existing trees in the model are not changed. A gradient descent procedure is used to minimize the loss when adding trees.

GBT builds trees one at a time, where each new tree helps to correct errors made by previously trained trees. A great application of GBM is anomaly detection in supervised learning settings where data is often highly unbalanced such as DNA sequences, credit card transactions or cybersecurity.

### Decision Tree Classifier

In decision analysis, one of the visual and explicit representations of decision and decision making procedure can be performed by using decision trees. The decision tree is using a tree-like model of decisions such that a flowchart like tree structure in which each node internal node denotes a test and each branch represents an outcome of the test and each leaf node holds a corresponding class label.

### Logistic Regression

When working with our data that accumulates to a binary separation, we want to classify our observations as the customer “will churn” or “won’t churn” from the platform. A logistic regression model will try to guess the probability of belonging to one group or another. The logistic regression is essentially an extension of a linear regression, only the predicted outcome value is between [0, 1]. The model will identify relationships between our target feature, Churn, and our remaining features to apply probabilistic calculations for determining which class the customer should belong to. We will be using the ‘ScikitLearn’ package in Python.

<img width="916" alt="image" src="https://github.com/AAKASHKSHETTY/Big_Data_Music_Churn_Prediction/assets/58876667/48f8198f-f61a-43fe-826f-db41a6cc9cd2">

## Results

Machine learning modeling has a success in predicting the customer's churn activity. That can help the application owners to improve the user lifetime. Despite the relatively good results of other selected machine learning models such as SVM and Decision Trees, the GBT algorithm is selected and tuned with hyperparameters. For future work, the Random Forest algorithm can be adjusted with different settings to overcome the over-fitting.

<img width="935" alt="image" src="https://github.com/AAKASHKSHETTY/Big_Data_Music_Churn_Prediction/assets/58876667/3df757a8-dfa3-4ade-a390-0084cb430cfd">

## Conclusion

In this project, we were able to study the service dataset and create functions for the modeling process. To begin with, we studied different levels of the dataset, which was the logs of each user session. The dataset allowed us to study churn and create suitable predictive features. In addition, feature selection was not a trivial task. We trained four models: Logistic Regression, Random Forest, Gradient Boosted Trees, and Decision Tree Classifier.
Feasibility and cost become very important in real applications. A model like this can be run weekly or monthly depending on the data latency, business requirements. Operational cost should be monitored and model results should be validated with testing (A/B testing). Experiment results(evaluation metrics, KPIs) should be tracked so that our model and following actions could bring value to business.
