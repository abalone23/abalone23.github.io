---
layout: post
title:  "How to Display All Columns and Rows in a Jupyter Notebook"
date:   2019-10-12 13:35:15 -0700
categories: machine_learning
tags: jupyter
---

In order to conserve space/memory, there is a limit to how many columns and rows that are displayed when viewing Pandas dataframes in Jupyter notebooks. However, when doing explatory data analysis (EDA), sometimes I want to view all columns or rows if it's not too big. To see what the defaults are (Pandas 0.25.1):

```python
pd.get_option("display.max_rows")
pd.get_option("display.max_columns")
```
![jupyter default]({{ site.url }}/assets/posts_images/pd-default-rowcols.png){:width="500px"; margin: auto;}

More details are explained in Pandas [Options and settings](https://pandas.pydata.org/pandas-docs/stable/user_guide/options.html).

To show what it looks like, I created a sample dataframe populated with zeros using the numpy function [arange](https://docs.scipy.org/doc/numpy/reference/generated/numpy.arange.html):
> Return evenly spaced values within a given interval.

```python
df = pd.DataFrame(0, index=np.arange(100), columns=np.arange(30))
```

![jupyter default]({{ site.url }}/assets/posts_images/jupyter-colrows-default.png){:width="500px"}

To override these settings (Pandas 0.25.1):

```python
# display all columns
pd.options.display.max_columns = None

# display all rows
pd.options.display.max_rows = None
```

![jupyter default]({{ site.url }}/assets/posts_images/jupyter-colrows-all.png){:width="500px"}