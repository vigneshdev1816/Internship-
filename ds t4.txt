import tweepy
import nltk
from nltk.sentiment.vader import SentimentIntensityAnalyzer
import matplotlib.pyplot as plt

# Twitter API credentials
consumer_key = 'your_consumer_key'
consumer_secret = 'your_consumer_secret'
access_token = 'your_access_token'
access_token_secret = 'your_access_token_secret'

# Set up Twitter API connection
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth)

# Define the topic or brand to analyze
topic = '#PythonProgramming'

# Collect tweets related to the topic
tweets = tweepy.Cursor(api.search, q=topic, lang='en').items(100)

# Initialize sentiment analyzer
sia = SentimentIntensityAnalyzer()

# Initialize lists to store sentiment scores and tweet dates
sentiment_scores = []
tweet_dates = []

# Analyze sentiment of each tweet
for tweet in tweets:
    sentiment_score = sia.polarity_scores(tweet.text)['compound']
    sentiment_scores.append(sentiment_score)
    tweet_dates.append(tweet.created_at)

# Visualize sentiment patterns over time
plt.plot(tweet_dates, sentiment_scores)
plt.xlabel('Tweet Date')
plt.ylabel('Sentiment Score')
plt.title('Sentiment Patterns for #PythonProgramming')
plt.show()