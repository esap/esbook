# 3.7 批量设置登录中断已有连接
![](./3.7.jpg)  
SQL代码：  
```sql
update es_user
   set synlogin=0 --设置不可同时登陆
go
update es_userOption
   set exitOnNewLogin=1 --设置登录后中断已有连接
```
