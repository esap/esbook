# 3.3 库存月报表示例
```sql
create table table1(产品代号 varchar(10),产品名称 varchar(10),产品型号 varchar(10),产品规格 varchar(10),时间 datetime,入库数量 int,单价 decimal(10,2),库存剩余量 int,单位 varchar(10))
insert table1 select '1002','ab','bc','dd','2004-08-21',200,632,200,'件'
union  all    select '1001','aa','bb','cc','2004-09-01',100,526,83 ,'台'
union  all    select '1001','aa','bb','cc','2004-08-01',100,526,83 ,'台'

create table table2(产品代号 varchar(10),产品名称 varchar(10),产品型号 varchar(10),产品规格 varchar(10),时间 datetime,出库数量 int,单价 decimal(10,2),单位 varchar(10))
insert table2 select '1001','aa','bb','cc','2004-09-11',17,526,'台'
go

select * from table1
select * from table2

/*
目的：
根据这两个表中数据生成“库存月报表”
代号、名称、型号、规格、单位、
上月结存数量、单价、金额
本月收入数量、单价、金额
本月发出数量、单价、金额
本月结存数量、单价、金额
*/
select
	年月=isnull(a.年月,b.年月)
	,产品代号=isnull(a.产品代号,b.产品代号)
	,产品名称=isnull(a.产品名称,b.产品名称)
	,产品型号=isnull(a.产品型号,b.产品型号)
	,产品规格=isnull(a.产品规格,b.产品规格)
	,单位=isnull(a.单位,b.单位)
	,上月结存数量=isnull(a.上月结存数量,0)-isnull(b.上月结存数量,0)
	,上月结存单价=case 
		when a.上月结存单价 is null and b.上月结存单价 is null
		then 0
		else (isnull(a.上月结存单价,0)+isnull(b.上月结存单价,0))
		/(case when a.上月结存单价 is null then 0 else 1 end
		+case when a.上月结存单价 is null then 0 else 1 end)
	end
	,上月结存金额=isnull(a.上月结存金额,0)-isnull(b.上月结存金额,0)
	,本月收入数量=isnull(a.本月收入数量,0)
	,本月收入单价=isnull(a.本月收入单价,0)
	,本月收入金额=isnull(a.本月收入金额,0)
	,本月出库数量=isnull(b.本月出库数量,0)
	,本月出库单价=isnull(b.本月出库单价,0)
	,本月出库金额=isnull(b.本月出库金额,0)
from
	(
		select
			年月=convert(varchar(7),时间,120)
			,产品代号,产品名称,产品型号,产品规格,单位
			,上月结存数量=(select sum(入库数量) from table1 where 产品代号=a.产品代号 and 时间<min(a.时间))
			,上月结存单价=(select avg(单价) from table1 where 产品代号=a.产品代号 and 时间<min(a.时间))
			,上月结存金额=(select sum(入库数量*单价) from table1 where 产品代号=a.产品代号 and 时间<min(a.时间))
			,本月收入数量=sum(入库数量)
			,本月收入单价=avg(单价)
			,本月收入金额=sum(入库数量*单价)
		from
			table1 a
		group by
			convert(varchar(7),时间,120)
			,产品代号,产品名称,产品型号,产品规格,单位
	)a
full join
	(
		select
			年月=convert(varchar(7),时间,120)
			,产品代号,产品名称,产品型号,产品规格,单位
			,上月结存数量=(select sum(出库数量) from table2 where 产品代号=a.产品代号 and 时间<min(a.时间))
			,上月结存单价=(select avg(单价) from table2 where 产品代号=a.产品代号 and 时间<min(a.时间))
			,上月结存金额=(select sum(出库数量*单价) from table2 where 产品代号=a.产品代号 and 时间<min(a.时间))
			,本月出库数量=sum(出库数量)
			,本月出库单价=avg(单价)
			,本月出库金额=sum(出库数量*单价)
		from
			table2 a
		group by
			convert(varchar(7),时间,120)
			,产品代号,产品名称,产品型号,产品规格,单位
	)b
on a.产品代号=b.产品代号 and a.年月=b.年月
order by isnull(a.产品代号,b.产品代号),isnull(a.年月,b.年月)
go

drop table table1
drop table table2
```

### 本节贡献者
*@毛毛*  
