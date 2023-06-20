# ChurnPrediction

Streamify is a music streaming service ( like Spotify, Pandora, etc. ). In Sparkify users can either listen to music for free or buy a subscription. The free users will listen to ads while the subscription users ( or paid users ) will listen to songs ad-free. Users need a login to listen to a song on the service.
From a subscription perspective at any point, users can do any of the following: 
1. Upgrade from the free tier to the paid tier
2. Downgrade from the paid tier to the free tier
3. Cancel their account and leave the service
We want to identify the users that could potentially cancel their account and leave the service. By identifying this population upfront, we could incentivize them by giving them discounts or other rewards. This could make them stick, giving us a loyal customer base which is key for a company’s growth.

<img width="695" alt="image" src="https://user-images.githubusercontent.com/58876667/212476880-630b6f8e-78e9-4942-8785-1362912b6fef.png">

We have built an end to end data pipeline starting with data streaming from a log file emulating the application backend to the Kafka cluster. There will be a dedicated Spark streaming application to consume the stream message and ingest to local file storage emulating Hadoop, stored as json files, and retrieved with Spark Dataframe API via Jupyter to perform Churn Prediction analysis. The experiment focuses on analyzing and predicting the factors impacting churn rate.

## Kafka Streaming

The kafka producer server parses the json log file and prepares the message to be sent over as a stream of data. The app.cfg lists out parameters used in the application (specific for Kafka and Spark). The kafka producer notebook contains the logic to send the kafka message. A sample kafka consumer notebook is used for testing the message sent by the kafka producer. In the Spark streaming notebook,weconsumethe messagefromKafka,transformandstoreonlocalfile storage.
