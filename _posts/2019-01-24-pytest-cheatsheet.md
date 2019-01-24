---
layout: post
title: "Pytest - My Cheat List"
categories:
  - cheatlist
tags:
  - en
  - python
  - pytest
  - cheatlist
  - cheastsheet
featured-img: list
last_modified_at: 2019-01-24T14:25:52-05:00
---

# Summary

* [Pytest minimum example](#pytest-basics)
* [Parametrize example](#parametrize)
* [Assert raises an error](#assert-raises)
* [Basic example of fixtures](#fixtures)


<h2 id='pytest-basics'>Pytest minimum example</h2>

```python
# test_basic.py

def test_something():
    assert True
```

<h2 id='parametrize'>Parametrize example</h2>

Creating multiple tests with a single function

```python
import pytest

def is_even(input):
    if input % 2 == 0:
        return True
    return False

@pytest.mark.parametrize("input,expected", [
    (2, True),
    (3, False),
    (11, False),
])
def test_is_even(input, expected):
    assert is_even(input) == expected
```

<h2 id='assert-raises'>Assert raises an error</h2>

```python
import pytest

def do_something(input):
    if input == 0:
        raise ValueError('A very specific bad thing happened.')
    return True

def test_do_something():
    with pytest.raises(ValueError):
        do_something(0)
```

<h2 id='fixtures'>Basic example of fixtures</h2>

```python
import pytest

@pytest.fixture
def user():
    return {
        'name': 'John Snow',
        'email': 'john@snow.wes'
    }

def test_do_something(user):
    assert user['name'] == 'John Snow'
```
