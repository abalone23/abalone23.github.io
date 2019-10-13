---
layout: post
title:  "How to Display All Columns and Rows in a Jupyter Notebook"
date:   2019-10-12 13:35:15 -0700
categories: machine_learning
tags: jupyter
---

Sometimes when working with Jupyter notebooks not all rows and columns are displayed.

```python
df = pd.DataFrame(0, index=np.arange(100), columns=np.arange(30))
```

![jupyter default]({{ site.url }}/assets/posts_images/jupyter-colrows-default.png){:width="500px"}

To override these settings (Pandas 0.25.1):

```python
pd.options.display.max_columns = None # display all columns
pd.options.display.max_rows = None # display all rows
```

![jupyter default]({{ site.url }}/assets/posts_images/jupyter-colrows-all.png){:width="500px"}