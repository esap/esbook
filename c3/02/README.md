# 3.2 SQL触发器应用示例

### 回写采购订单状态
> @家里地位第三：  
![](./3.2.1.jpg?raw=true)

### 解决ES版本号丢失问题  
> @XゅY 
![](./3.2.2.jpg?raw=true)

### 解决ES用户密码批量丢失问题(8.4+版本可能发生)
> @傻木

在ES-USER 表单增加UserPwd1字段，设置触发器

```sql
BEGIN
UPDATE es_user
SET userpwd = userpwd1
WHERE
	userpwd <> userpwd1
```

将正确的密码填在UserPwd1字段即可

ES密码是将明文转换为MD5值的，更改密码时使用MD5值计算器：例如用户密码为123

那么需要在	UserPwd1字段填写 202CB962AC59075B964B07152D234B70

![](./3.2.4.jpg?raw=true)

### 更新采购订单状态
> @Kang
```sql
/*
采购订单结案  2016-4-15
新建采购入库单,更新采购订单已交数量.
当该采购单中所有物品交齐后，触发器更新主表的状态字段
*/
Create trigger 采购单结案 on 采购订单M                  
For Update   
AS
IF UPDATE(已交数量)
BEGIN
     UPDATE 采购订单M SET 状态=(CASE WHEN ISNULL(已交数量,0)=ISNULL(数量,0) THEN 1 ELSE 0 END)
END
BEGIN
     SET NOCOUNT ON;
     UPDATE 采购订单Z SET 状态=CASE WHEN A.CLOSED=1 THEN '未完成' ELSE '已完成' end
     FROM
	(SELECT EXCELSERVERRCID,MAX(CASE WHEN ISNULL(状态,0)=0 THEN 1 ELSE 0 END) CLOSED
	 FROM 采购订单M
	 WHERE EXCELSERVERRCID IN (SELECT EXCELSERVERRCID FROM INSERTED
		                   UNION 
		                   SELECT EXCELSERVERRCID FROM DELETED)
	GROUP BY EXCELSERVERRCID
	) A
     WHERE 采购订单Z.EXCELSERVERRCID=A.EXCELSERVERRCID             
END
```

### 工作台防止删单导致负库存示例( asked by @昆明haotian )
> @村长
```sql
create trigger ck on io --io是入库单主表，ios是入库单明细
instead of delete
as 
begin
declare @ct int
select @ct=(select COUNT(*)  --TODO:如果明细允许多个料号，则需要用sum函数聚合qty并按id分组
	from deleted,ios,库存 
	where deleted.ExcelServerRCID=ios.excelserverrcid and ios.id=库存.id and ios.qty>库存.qty)
if (@ct>0)
raiserror('库存不允许为负！',16,1)
end
```

效果图：

![](./3.2.3.jpg?raw=true)


### 关帐并锁定所有非盘点单的收发单据(除盘点外禁止增删改)
设置一个关帐控制台，建一个模板，设置一个包含 `关帐flag` 的整数型字段的表 `S`。

![](./3.2.7.jpg?raw=true)

在收发单据主表 `IO` 表上建触发器，代码如下：

> @村长
```sql
create trigger ck on io --io是入库单主表
for insert,update,delete
as 
begin
	if ((select 关账flag from s)=1 )  --从S表扫描关帐状态: 1表示开账, 2表示关账
		return 
	else if ((select io类型 from inserted) <> '盘点')  --IO表上有个字段叫IO类型，取值为：采购入库，生产入库，生产领料，盘点等。
		raiserror('关账期间只能盘点，其他出入库冻结！',16,1)
	else if ((select io类型 from deleted) <> '盘点')
		raiserror('关账期间只能盘点，其他出入库冻结！',16,1)
end
```

效果图：

工作台删除非盘点出入单据时：

![](./3.2.5.jpg?raw=true)

表单增改保存时：

![](./3.2.6.jpg?raw=true)
