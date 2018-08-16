---
title: python sorted compare
date: 2018-07-31 10:49:59
tags: python
categories: python
description: how to sorter faster
---
## MySortData
Now that I have some data and need to sort it, I need to find a faster sorting method in the sorting method.Data as shown below:
```python
data = {1:{'total_score':50, 'compare_score':49, 'name':'roc'},.....}
```

The above is a set of data, and I need to sort them according to the score fieldin the dictionary
## Compare sorted() obj.sort()
I try sorted() Methon:
```python
result = sorted(data.items(), key=lambda x:x[1]['total_score'], reverse=True)

```

OK! I get the result!  it's average run time is : 7.5841E-06
And I try obj.sort() Methon:
```python
result.sort(key=lambda x:x['compare_score'], reverse=True)
```

it's average run time is : 6.5666E-06
This result doesn't seem to make any difference
## Use itemgetter 

Because of my depth of listing, I can't use itemgetter directly. So first define it
```python
get_position = itemgetter(1) # get index 1
get_compare = itemgetter('compare_score') # get compare_score data
get_total = itemgetter('total_score') # get total_score data
```

Now use sorted():
```python
result = sorted(data.items(), key=lambda x: get_total(get_position(x)), reverse=True)
```
Average run time: 1.18327E-05
??????????????????

See sort():
```python
result.sort(key=lambda x: get_compare(get_position(x)), reverse=True)
```
Average run time: 1.01018E-05

##  Summary 
From the above results, we can see that there is not much difference between using sorted () and using obj. sort (), and obj. sort () may be a little bit faster. But the speed difference with itemgetter is very large.

