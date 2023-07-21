---
layout: post
title:  "Reddit Sentiment Analysis"
date:   2023-07-19 00:31:53 -0600
categories: jekyll update
---
Enter a subreddit and perform sentiment analysis. Returns the percentage of positive, negative, and neutral headlines in a subreddit. Also displays the five most positive and the five most negative subreddits. See the Github Repo <a href="https://github.com/cullena20/RedditSentimentWebsite" target="_blank"> here</a>. See the Colab Notebook <a href="https://colab.research.google.com/github/cullena20/RedditSentiment/blob/main/RedditSentiment.ipynb"> here</a>.
&nbsp;

![Reddit Sentiment Screenshot](/assets/images/projects/RedditSentiment.jpeg)

<!-- excerpt-end -->
&nbsp;

I made this project over the course of a weekend for a Hackathon in 2020. It was one of my first larger computer science projects. I learned a lot from this project. It was my first time creating a website with backend, working with a large website's API and data, applying ML models on data from the wild, and deploying a website on Heroku (unfortunately the Heroku website is no longer available because Heroku stopped offering
free dynos).

# How does this work? 
A natural language processing model is used to determine the sentiment of a subreddit from the top
50 newest titles. The user is able to type a subreddit, which will then be ran through the machine
learning model. The model classifies titles as positive, negative, or neutral and returns the
percentage breakdown of a subreddit, along with the top five most positive and top five most
negative posts.

# Methodology for Nerds 
Our first step was to explore the Reddit API and to examine our data. We then had to figure out
the best machine learning model for the task. We ended up using VADER, a pretrained model from NLTK that specializes in identifying the sentiment of social media text. We tested this model
on several subreddits and explored the results, before coding this into a python script, and then
into a website. To see more about the model building process, check out the interactive Colab
notebook.

# Future Steps
Using this same user interface, we could test out different models that may bring us more informative
analyzes of subreddits. For example, we could train a model that could detect fake news or hate speech
and give scores for subreddits. Additionally, we could change our methodology in deploying our model on
subreddits. Instead of looking at titles, it may be more informational to take into account comments or
even upvotes.