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
The data that was used comes from the 2017 American Community Survey. It can be found on [IPUMS USA](https://usa.ipums.org/usa/).

> IPUMS provides census and survey data from around the world integrated across time and space. IPUMS integration and documentation makes it easy to study change, conduct comparative research, merge information across data types, and analyze individuals within family and community context. Data and services available free of charge.

This is an approximately three million observation, weighted sample dataset that represents the United States population.

# Exploratory Data Analysis (EDA)
Most data was categorical such as marital status, English ability and education. I removed outliers and children, and binned age and income data based on low, middle and upper income for the income feature, and young adult, adult and senior for the age feature.

{% highlight python %}
df = df[(df['ftotinc'] < 9999999) & (df['ftotinc'] >= 0)]
df = df[(df['age'] < 120) & (df['age'] >= 18)]

df['incbrack'] = pd.cut(df['ftotinc'], [-1, 40000, 90000, 3000000], labels=['low_income', 'middle_income', 'upper_income'])

df['agebrack'] = pd.cut(df['age'], [17, 25, 64, 120], labels=['young_adult', 'adult', 'senior'])
{% endhighlight %}

Since most of the categorical features contained more categories than necessary, I narrowed down the scope. For example, the
marital status feature, `MARST`, contained:
> 1-Married, spouse present 
> 2-Married, spouse absent 
> 3-Separated 
> 4-Divorced 
> 5-Widowed 
> 6-Never married/single

I grouped `Married, spouse absent` into the `Married, spouse present` category, `Separated` to `Divorced`, and `Widowed` to `Never married/single`

{% highlight python %}
df['marst'] = df['marst'].replace([2, 3, 5], [1, 4, 6])
df['marst'] = df['marst'].replace([6, 1, 4], ['single', 'married', 'divorced'])
{% endhighlight %}

As a final step, I converted the categorical features using [dummy variables](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.get_dummies.html) so it can be used in the classification model.

# Model
Once the features were binned and I tried Random Forest, Logistic Regression and XGBoost classification models. For Random Forest and Logistic Regression, I used the `class_weight='balanced'` option and for XGBoost I used SMOTE oversampling. Although XGBoost performed slightly better based on a higher ROC/AUC score, which tells how much a model is capable of distinguishing between classes such that the higher the score is a good measure of separability, I decided to use Logistic Regression for it's interpretability.

For the Logistic Regression model, I performed hyperparameter tuning using `GridSearchCV` with `class_weight='balanced'`. I also made sure to fit the model with the sample_weight option due to the dataset being weighted. I used the recall score to determine the best C parameter to use. C is described in the [sklearn doc](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html) as:
> Inverse of regularization strength; must be a positive float. Like in support vector machines, smaller values specify stronger regularization.

# The App
![hcbtn app]({{ site.url }}/assets/hcbtn-graph.png)
The app, [healthcarebythenumbers.com](https://www.healthcarebythenumbers.com), is Flask-based and uses [SQL Alchemy](https://www.sqlalchemy.org/), an "open-source SQL toolkit and object-relational mapper for the Python" that connects to a [PostgreSQL](https://www.postgresql.org/) database.

In addition to displaying a state-by-state breakdown of uninsured rates among various groups, it also features a [prediction calculator](https://www.healthcarebythenumbers.com/predict) where one can enter various demographic characteristics to predict the likelihood of having healh care insurance.

[Github](https://github.com/abalone23/hcbtn)