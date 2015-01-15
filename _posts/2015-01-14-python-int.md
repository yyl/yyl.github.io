---
layout: post
title:  "Pandas notes"
date:   2015-01-06 23:10:00
categories: pandas
---

Here I note down some tips and snippets took me some time to figure out.

### 1. add new data to a dataframe

Here I want to add new data (a row or several rows) to an existing dataframe.

To add one new row of data, it first has to be a dictionary.

{% highlight python %}
new_row = {'userid':[user], 'method':[method1], 'duration':[None]}
appended_df = df.append(pd.DataFrame(new_row), ignore_index=True)
{% endhighlight %}

In the `new_row` variable, the key is the column name. When appending single row, one needs to make every value a list to avoid [TODO] error. Put `None` if the value of a certain column does not exist in this row.

To add several rows, one could repeat the above procedure, or add them in one batch using a list

{% highlight python %}
new_data = []
new_row1 = {'userid':[user], 'method':[method1], 'duration':[None]}
new_row2 = {'userid':[user2], 'method':[method2], 'duration':[None]}
new_data.append(new_row)
appended_df2 = df.append(pd.DataFrame(new_data), ignore_index=True)
{% endhighlight %}

Note `append` returns the new appended dataframe, therefore one has to assign it to a new variable.

_TBD_