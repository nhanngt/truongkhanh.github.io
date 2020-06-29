---
title: "How to check data in dict before reading it"
date: 2020-06-29T14:00:00
categories:
    - programming
tags:
    - python
    - dict
---

There is a new Java program producing events in Json format and a Python program consumes those events.
However, after the integration, there are bunch exceptions in several places including
- 'NoneType' is not iterable
- 'NoneType' object has no attribute 'get'

It turns out that by default,Java Jackson json serialization includes fields with null value while in Python program, 
the code to access data is as below:
```python
def do_something(value):
  # expect value is not None
  pass

# method 1
value = data.get('key', {})
do_something(value) # do_something expect value is not None

# method 2
if 'key' in data:
    do_something(data['key'])
```

Both two methods above will cause exception if `data = {'key': None}`.

Actually, the safer way to do is
```python
value = data.get('key') or {}
do_something(value)
```