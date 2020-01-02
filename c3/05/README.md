# 3.5 不断号自动编号函数
```sql
CREATE FUNCTION [dbo].[GetQuoID](@headStr nvarchar(10),@date datetime)
RETURNS nvarchar(50)
BEGIN 
declare  @oid2 nvarchar(50)
declare @oid nvarchar(50)
declare @month nvarchar(2)
declare @year nvarchar(4)
declare @ym nvarchar(6)
set @month=month(@date)
if len(@month)=1
    set @month='0'+@month --使月为两位长
set @year=right(convert(nvarchar,year(@date)),2)--使年为两位长
set @ym=@year+@month --组成年月字符

--格式Q200908001
if exists(select * from 销售单_主表)
begin
    select  top 1 @oid2=单据编号 
	from 销售单_主表 
	order by EXCELSERVERRCID desc --获取最后一条的单据编号，一定要有id，并且自动生成的，倒排序
end
else 
begin
    set @oid2=@headStr+@ym+'0000' --没有记录是默认为今天
end

--订单不是本月的，重新开始一个新的单据流水号
if convert(nvarchar,left(@oid2,7))<>@headStr+@ym
begin
--用本月的年月号开始
    set @oid2=@headStr+@ym+'0000'
end

declare @str nvarchar(50) --临时单号

set @str=convert(nvarchar,(convert(int,right(@oid2,4))+1)) --订单号加一
while (4-len(@str)>0)
begin
     set @str='0'+@str    
end
set @oid2=@headStr+@ym+@str
--print @oid2

--如果该编号已经存在，则重新获取
while exists(select * from 销售单_主表 where 单据编号=@oid2)
begin
    
    set @str=convert(nvarchar,(convert(int,right(@oid2,4))+1)) --编号加一
    while (4-len(@str)>0)
    begin
         set @str='0'+@str    
    end
    set @oid2=@headStr+@ym+@str
--     print @oid2
end

set @oid=convert(nvarchar,@oid2)
--print 'Q'+convert(nvarchar,year(getdate()))+convert(nvarchar,month(getdate()))+@str

RETURN @oid
END

----------------简易版本---------------------  
--只需要在触发器里加前缀就好了  
Create Trigger [dbo].[table_InsertTrigger] on [dbo].[销售单_主表]
for insert
as
declare @MAXID NVARCHAR(15)
select @MAXID= ExcelServerRCID from inserted
update 销售单_主表 set 单据编号=([dbo].[GetQuoID]('KD',getdate())) where ExcelServerRCID = @MAXID
```

### 本节贡献者
*@訫訫*  
