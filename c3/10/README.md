# 3.10 勤哲提高提取考勤数据速度
>村长提示：这里的考勤系统是中控，ES通过外部数据源或视图访问其考勤数据，即CHECKINOUT表。

### 目的
提高考勤软件的读写速度,因为如果按原有数据库达到考勤信息达60万数据,并且不断增加.

### 方案
将手工新增一个表 CHECKINOUTBACKUP,此表来源数据是CHECKINOUT,当来源数据表进行新增数据时,在CHECKINOUTBACKUP也新增一条数据.并且此表只保留2个月的数据!

### 解决方法
#### 第一步
当CHECKINOUT新增数据时自动插入CHECKINOUTBACKUP数据，需要在CHECKINOUT上写一个触法器:
```sql
CREATE TRIGGER [dbo].[Checkinout_insert1] 
   ON  [dbo].[CHECKINOUT]
   AFTER INSERT
AS 
BEGIN
        SET NOCOUNT ON;

    -- Insert statements for trigger here
insert into CHECKINOUTBACKUP select * from inserted

END
```

#### 第二步
定时删除2个月之前的数据，需要设置SQL Server代理，建立一个`dele`作业。

```sql
DELETE FROM [kaoqin].[dbo].[CHECKINOUTBACKUP]
      WHERE CHECKTIME < DATEADD(mm, - 2, CONVERT(nvarchar(10), GETDATE(), 120))
```

![](./3.10.1.jpg)
在勤哲中用外部数据源进行提数

### 本节贡献者
*@路柏*  
2016/12/13  
