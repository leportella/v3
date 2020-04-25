---
layout: post
title: "SQL - My Cheat Sheet"
categories:
  - english
  - cheatsheet
tags:
  - en
  - python
  - pytest
  - cheatlist
  - cheastsheet
  - python
  - community 
  - pyladies
  - technology
  - tecnologia
  - programador
  - programadora
  - developer
  - mulheres na tecnologia
  - woman in tech
  - girls in tech
  - computação
  - ciência de computação
  - software development
  - software engineering
  - engenharia de software
  - desenvolvimento
  - auto-ensino
  - self-taught engineer
  - code
  - Django
  - software
  - career
  - tech career
  - open-source
  - no cs degree
  - cs
  - computer science
  - sql
  - database
featured-img: search
permalink: cheatsheet-sql.html
last_modified_at: 2020-04-25T14:25:52-05:00
---

# Summary

* [Query if a string is in a list](#query-list)
* [Query within a timestamp](#timestamp)
* [Creating temporary table](#temp-table)
* [String comparison](#string-comparison)
* [Search for empty or null strings](#empty-string)


<h2 id='query-list'>Query if a string is in a list</h2>

```sql
select name, 
    from my.table
where name IN ('jon', 'mary')
```


<h2 id='timestamp'>Query within a timestamp</h2>

```sql
select record
    from my.table
where
    created > timestamp '2019-10-01 00:00' AND
    created < timestamp '2019-10-01 23:59'
```

<h2 id='temp-table'>Creating temporary table</h2>

```sql 
with my_new_table as (
  -- something here
  select *
  from another.table
)

select *
from my_new_table
```

<h2 id='string-comparison'>String comparison</h2>

```sql
select
    *
from my_table
where my_table.items LIKE '%something%'
limit 10
```

<h2 id='empty-string'>Search for empty or null strings</h2>

```sql
where (surname is null or surname = '')
```