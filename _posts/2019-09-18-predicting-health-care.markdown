---
layout: post
title:  "Predicting the Likelihood of having Health Care Insurance"
date:   2019-09-18 09:35:15 -0700
categories: classification, machine learning
---
![Healthcare]({{ site.url }}/assets/natasha-spencer-_hH0dC6A-FM-unsplash.jpg)
Photo by [Natasha Spencer](https://unsplash.com/@totalshape?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on Unsplash

# Background
Health care insurance can be found in both the public and private marketplace and due to financial constraints some Americans are uninsured as well. Lack of health insurance results in worse preventive care and services for major health conditions and chronic diseases. By using a classification model on a sample US population from 2017 I analyzed demographic characteristics to find which populations are most at risk for not having health insurance, and use this information in a classification model to predict the likelihood of someone having health insurance based on demographic features they provide.

# Data
The data that was used comes from the 2017 American Community Survey. This is a 3 million row, weighted sample dataset that represents the United States population.

# Exploratory Data Analysis (EDA)
Most data was categorical such as marital status, English ability and education. I binned age and income data.

{% highlight python %}
df = df[(df['ftotinc'] < 9999999) & (df['ftotinc'] >= 0)]
df = df[(df['age'] < 120) & (df['age'] >= 18)]
{% endhighlight %}

# Model
I tried Random Forest, Logistic Regression and XGBoost classification models. For Random Forest and Logistic Regression, I used the `class_weight='balanced'` option and for XGBoost I used SMOTE oversampling. ALthough XGBoost performed slightly better based on a higher ROC/AUC score, which tells how much a model is capable of distinguishing between classes such that the higher the score is a good measure of separability, I decided to use Logistic Regression for it's Interpretability.

For the Logistic Regression model, I performed HYperParameter tuning usingm `GridSearchCV` with `class_weight='balanced'`. I also made sure to fit the model with sample_weight option due to the dataset being weighted.

# The App
The app, [healthcarebythenumbers.com](https://www.healthcarebythenumbers.com), contains a form where one can enter various demographic characteristics which is used to predict the likelihood of having healh care insurance. It also displays uninsured rates and the characteristics associated with being uninsured for each state.