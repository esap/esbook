# 1.3 ES常见问题汇总

## 1. 196790错误
![](./1.3.1.jpg) 
   
装了勤哲无法识别的WPS

> @ゝ莫说：WPS有个美化插件，会冲突，有的账号可以登有的登不了。删掉WPS后正常，装上完蛋 。

## 2. 197722错误
![](./1.3.2.jpg)  
解决方法1：卸载，重新安装，用管理员身份  
解决方法2：在系统环境变量中添加ES安装路径如：C:\Program Files (x86)\Cesoft\Excel Server2010

## 3. 70错误
![](./1.3.3.jpg)  
win7安装勤哲时右击以管理员权限运行
> ES很依赖管理员，安装如此，运行也如此

## 4. 53错误
![](./1.3.4.jpg)
![](./1.3.5.jpg)  
2013版本:360禁用了某些组件，360弹出的时候点同意就行了。
> 真爱生命，远离360	

## 5. 40402错误
![](./1.3.6.jpg)  
直接调数据库大小，win2003要装sql2000企业版,装个人版数据库大小不可调

![](./1.3.5.1.png) 

> 路柏 解决方法如下

两台服务器是WIN2008,数据库是SQL2005，两台服务器都需要进行设定以下的步骤

1：在控制面板-打开管理工具--组件服务

![](./1.3.5.2.png)

第一步打开组件服务按以下图设定

![](./1.3.5.3.png)

![](./1.3.5.4.png)

![](./1.3.5.5.png)

按确定之后，会问是否需要重启，解决问题

## 6. 勤哲端口
|端口|类型|
|:---------|:--:|
|7777      |软件|  
|7778,7779 |网盘|
|80        |web|

## 7. win2003iis开了后自动关闭.
点击“开始”-“控制面板”-“管理工具”-“组件服务”-“计算机”-“我的电脑”-“DCOM”选项， 选择其下的“IIS ADMIN SERVICE”，右健选择“属性”，找到“安全”，在“启动和激活权限”中编辑“自定义”，添加帐号“NETWORK SERVICE ”

## 8. 忘记系统管理台密码了
![](./1.3.7.jpg)  
进数据库ESsystem,找到表SysInfo里的sysPwd删了，然后登陆，系统会提示没密码

如果是应用的admin密码忘了，可以进对应应用的数据库找到user表的userPwd字段清空，即可免口令登陆

## 9. 图片发生意外错误
![](./1.3.8.jpg)  
Internet选项-程序-设为默认值

## 10. 勤哲自带人民币大写函数丢失（ESF_MnvToCape）
![](./1.3.9.jpg)  
此函数容易丢失，所以用excel自带的函数(web客户端没有测)：
	=IF(O18<0,"负","")&SUBSTITUTE(SUBSTITUTE(TEXT(TRUNC(FIXED(ABS(O18))),"[>0][dbnum2];;")&IF(INT(O18),"元",)&TEXT(RIGHT(FIXED(O18),2),"[dbnum2]0角0分;;"&IF(ABS(O18)>1%,"整",)),"零角",IF(ABS(O18)<1,,"零")),"零分","整")
> 拼音等函数也一样，能不用自带最好不用

## 11. XP是中文，win7中是英文
需修改wsweb文件夹下的webconfig.ini,将value="en-us"改为value="zh-cn"

## 12. 登录勤哲闪退，秒退
在excel里的禁用里把esclient10.dll启用就行了。  
2007版以上：  
![](./1.3.23.png)

## 13. Excel模版公式值变成数值1，公式失效。
![](./1.3.10.jpg)  
9.2.335出现过，解决方法：设计模版的时候不点保存，而是点关闭，弹出保存的时候再保存。这样公式就不会变成1了

## 14. win7X64位,web客户端填报卡在那不动了
![](./1.3.11.jpg)

## 15. 429错误(未完全解决)
据说要注册 msdtcprx.dll CoESDBAdapter.dll  
![](./1.3.12.jpg)  
- 解决方法1：运行	`regsvr32 scrrun.dll`
- 解决方法2：运行	`regsvr32 "C:\Program Files\Common Files\System\ado\msado15.dll"`
- 解决方法3：如果上述两法仍未解决，则可能是其它原因，不仿试试下面bat批处理代码。  
将下面代码复制到记事本，另存为 cmd.bat，双击运行就可以了。  
```sh
echo 正在修复，这个过程可能需要几分钟，请稍候…… 
rundll32.exe advpack.dll /DelNodeRunDLL32 %systemroot%\System32\dacui.dll 
rundll32.exe advpack.dll /DelNodeRunDLL32 %systemroot%\Catroot\icatalog.mdb 
regsvr32 /s comcat.dll 
regsvr32 /s asctrls.ocx 
regsvr32 /s oleaut32.dll 
regsvr32 /s shdocvw.dll /I 
regsvr32 /s shdocvw.dll 
regsvr32 /s browseui.dll 
regsvr32 /s browseui.dll /I 
regsvr32 /s msrating.dll 
regsvr32 /s mlang.dll 
regsvr32 /s hlink.dll 
regsvr32 /s mshtml.dll 
regsvr32 /s mshtmled.dll 
regsvr32 /s urlmon.dll 
regsvr32 /s plugin.ocx 
regsvr32 /s sendmail.dll 
regsvr32 /s mshtml.dll /i 
regsvr32 /s scrobj.dll 
regsvr32 /s corpol.dll 
regsvr32 /s jscript.dll 
regsvr32 /s msxml.dll 
regsvr32 /s imgutil.dll 
regsvr32 /s cryptext.dll 
regsvr32 /s inseng.dll 
regsvr32 /s iesetup.dll /i 
regsvr32 /s cryptdlg.dll 
regsvr32 /s actxprxy.dll 
regsvr32 /s dispex.dll 
regsvr32 /s occache.dll 
regsvr32 /s iepeers.dll 
regsvr32 /s urlmon.dll /i 
regsvr32 /s cdfview.dll 
regsvr32 /s webcheck.dll 
regsvr32 /s mobsync.dll 
regsvr32 /s pngfilt.dll 
regsvr32 /s licmgr10.dll 
regsvr32 /s hhctrl.ocx 
regsvr32 /s inetcfg.dll 
regsvr32 /s trialoc.dll 
regsvr32 /s tdc.ocx 
regsvr32 /s MSR2C.DLL 
regsvr32 /s msident.dll 
regsvr32 /s msieftp.dll 
regsvr32 /s xmsconf.ocx 
regsvr32 /s ils.dll 
regsvr32 /s msoeacct.dll 
regsvr32 /s wab32.dll 
regsvr32 /s wabimp.dll 
regsvr32 /s wabfind.dll 
regsvr32 /s oemiglib.dll 
regsvr32 /s directdb.dll 
regsvr32 /s inetcomm.dll 
regsvr32 /s msoe.dll 
regsvr32 /s oeimport.dll 
regsvr32 /s msdxm.ocx 
regsvr32 /s dxmasf.dll 
regsvr32 /s laprxy.dll 
regsvr32 /s l3codecx.ax 
regsvr32 /s acelpdec.ax 
regsvr32 /s mpg4ds32.ax 
regsvr32 /s danim.dll 
regsvr32 /s Daxctle.ocx 
regsvr32 /s lmrt.dll 
regsvr32 /s datime.dll 
regsvr32 /s dxtrans.dll 
regsvr32 /s dxtmsft.dll 
regsvr32 /s wshom.ocx 
regsvr32 /s wshext.dll 
regsvr32 /s vbscript.dll 
regsvr32 /s scrrun.dll mstinit.exe /setup 
regsvr32 /s msnsspc.dll /SspcCreateSspiReg 
regsvr32 /s msapsspc.dll /SspcCreateSspiReg 
echo. 
echo. 
echo 修复成功！任意键退出！ 
pause>nul
```

- 解决方法4:  
控制面板->管理工具->组件服务->计算机->我的电脑->DCOM配置  
找到Microsoft Excel 应用程序，右键它选择属性，然后查看一下安全选项卡，看一下是不中有权限进行“启动和激活权限”，“访问权限”，“配置权限”等，如果没有开放相应权限试试。

## 15. 工作台假死、无响应
登录后，工作台假死，无法操作系统  
（1）停掉客户机上的ESIMC.exe 进程  
（2）假如停掉ESIMC就没问题了，去到系统管理台上，勾掉开启消息提醒，重启服务，如果再没问题了，就可以确定是消息提醒的问题  
（3）用select * from es_imq，在essystem下执行  

## 16. 自动编号对应的SQL2000表
![](./1.3.13.jpg)  

## 17. 自定义查询不预警
第一次设置，要停服务，重起，就会有预警了

## 18. 326错误填报附件时报错
![](./1.3.14.jpg)
![](./1.3.15.jpg)  
winxp  +sql2000sp4
 
## 19. 如何查看Sql2000版本
## 查询分析器方法
#### 方法一 
第一步：在查询分析器中
```sql
select @@version
print @@version
```
执行，得到下面的信息
```sh
Microsoft SQL Server 2000 - 8.00.2039 (Intel X86) 
May 3 2005 23:18:38 
Copyright (c) 1988-2003 Microsoft Corporation 
Personal Edition on Windows NT 5.1 (Build 2600: Service Pack 2) 
```
另：在查询分析器中
```sql
SELECT SERVERPROPERTY('productversion'), SERVERPROPERTY ('productlevel'), SERVERPROPERTY ('edition')
```
运行结果如下： 
• 产品版本（例如，8.00.534）   
• 产品级别（例如，“RTM”或“SP2”）   
• 版本（例如，“Standard Edition”）。  
例如，运行结果可能类似于如下内容：  
```sh
8.00.534 SP2 Standard Edition
```
* 微软官方提供方法详见：http://support.microsoft.com/kb/321185/zh-cn 

#### 方法二
企业管理器信息：  
打开企业管理器－>SQL SERVRE 组－>(local)window NT ->属性  
产品:有personal的是个人版的，有Enterprise的是企业版的  
产品版本：8.00.2039(sp4);  

#### 方法三
软件版本：  
C:\Program Files\Microsoft SQL Server\MSSQL\Binn\sqlservr.exe   
点击鼠标右键查看版本也能得到，不过信息比较简单而已。8.00.2039就代表安装的SQL Server的版本了。  
SQL SERVER打补丁后的版本号Version Number Service Pack

	8.00.194 Microsoft SQL Server 2000
	8.00.384 Microsoft SQL Server 2000 SP1
	8.00.532 Microsoft SQL Server 2000 SP2
	8.00.760 Microsoft SQL Server 2000 SP3
	8.00.818 Microsoft SQL Server 2000 SP3 w/ Cumulative Patch MS03-031
	8.00.2039 Microsoft SQL Server 2000 SP4

## 20. 在管理工作表里设置分类显示，但在我的工作台不分类显示
![](./1.3.16.jpg)  
sql2000升级sp4后解决。
刚开始数据库很小的时候不会出现这个问题，当数据库达到20G以上就有可能出现这个问题。

## 21. 1053错误(未找到方法)
![](./1.3.17.jpg)

## 22. sqlserver日志清理
用trunc.exe清理
>  也可到SQL里压缩日志

## 23. 改es默认端口
![](./1.3.18.jpg)

## 24. 40411/40409错误
![](./1.3.19.jpg)
![](./1.3.20.jpg)  
填的了时候网络中断，再联上是可以保存报表的，但是如果换网络，就要重新登录
40409也有可能是服务程序因内存泄漏导致，要重启服务。

## 25. es2013.10.399工作流预警不提示(未解决)

## 26. win2008R2工作流待办预警在服务器上提示，在客户端上不提示(未解决)

## 27. 邮件设置，用qq邮箱时断时通(未解决)
推荐用263的邮箱作为服务邮箱。
> QQ，网易等免费邮箱容易被反垃圾策略阻挡，建议用企业邮箱

## 28. es不支持excel2013X64位(未解决)
装X32位.

## 29. ES装win2008R2+SQl2008R2卡顿
登录有4-5秒的卡，登录成功后点模版开始卡，后来不卡。  
装Sql2008R2时只勾选需要安装的部分。
> 确切的说，安装“数据库引擎+管理工具”即可，其他SDK等一般人用不到

## 30. ES2013.10.399装win2012+SQl2012退出系统时卡顿(未解决)
退出系统时有2-3秒的假死。

## 31. ES+WPS兼容性不好
WPS和excel并存。  
先安装excel+es,再装wps，提示wps为主要打开软件时取消勾选。
> WPS的兼容做得很差，还是建议用office

## 32. web字段完整显示  
![](./1.3.21.jpg?raw=true)  
修改配置文件  
![](./1.3.22.jpg?raw=true)  

## 33. 定时任务各种启动不了？
> @毛毛  
服务器要装iis，否则定时任务确实会启动不了，而且经常发神经，不正常  
要想正常，使用 sql server 代理

> @不笨的猪♂  
定时任务 模板要勾选支持web

定时任务例如自动邮件等容易内存泄漏，建议经常重启ES服务，例如每周末4点重启一次。

## 34. 保存时显示单元格超限
![](./1.3.23.jpg?raw=true)  

## 35. 模板打开时显示格式不对，导致模板打不开
调整控制面板-日期、时间、语言和区域设置-区域语言选项-自定义-时间-AM符号、PM符号改成英文AM\PM  
> win7下存在

## 36. .Net2.0与1.0并存时，如何注册2.0使ESWEB可用
```sh
cd C:\WINDOWS\Microsoft.NET\Framework\v2.0.50727
 aspnet_regiis.exe -i
```

## 37. 错误代码：-2147221504
![](./1.3.24.jpg)   
删除安装目录下ELD后缀的文件 

## 38. web上导出excel报表：选择路径失败
可信站点的activex都启用，在dos中注册ESCal_cln10.dll 如果还没解决，就把internet的activex都启用。

设置工具[允许ActiveX批处理](/files/允许ActiveX批处理.rar) by @简单生活

## 39. 自动邮件重复发送
检查ES_Server_Sec.exe进程是否有多个，关掉部分多余的。

## 40. The [image1]specified module could not be found
注册system32 下的msstdfmt.dll

## 41. 升级后一直提示：有部分文件因为正在被使用，没有更新成功，需要重启系统，是否现在就重启？
注册表中删除下面的键  
	HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run\ESAutoUpg

## 42. 打开表单时提示：此表单正被xxx打开

![](./1.3.25.jpg) 

进数据库执行以下代码
```sql
update es_repcase set openstate=0,openBy=0 where openBy=1
```

## 43. 网盘：提示组件注册未成功
* 2007及以上：在安装目录下执行regasm nfs_ui.dll /codebase  
* 若是03则安装补丁即可。

## 44. 由于本机限制问题
![](./1.3.26.jpg)   
修改注册表下 MACHINE/SOFTWARE/CLASSES/.html 及.htm  
默认值改回htmlfile
> @傻木-导入注册表即可  
	Windows Registry Editor Version 5.00
	
	[HKEY_CURRENT_USER\Software\Classes\.htm]
	@="Htmlfile"	
	
	[HKEY_CURRENT_USER\Software\Classes\.html]
	@="Htmlfile"
 

## 45. 用户已从其他机器登陆，不能再次登陆
![](./1.3.45.png) 

原因：这个是因为设置了不能多点同时登陆，而没勾选登陆后自动中断已有连接导致（后面这个选项经常会失效，应该算bug）

![](./1.3.45-2.jpg) 

解决方案：一般来说可以踢掉连接解决,如果碰巧admin也登不了，尝试用SQL解决

详见第三章：[批量设置登录中断已有连接](/c3/03.7.md) 

## 46. 错误代码：-2147181100
![](./1.3.46.png) 
> @benava
原因：es的弱bug，以前在本机比如有创建过账套名esapp1，后来删除了又创建同样的esapp1，可能是数据库里面删除不干净，导致里面的旧模板编码跟名称跟新账套有冲突，这个bug可大可小，找不出来，会让系统没办法用。   
解决方案：新建一个没有用过的账套，把旧账套备份然后导入过去就能解决。

## 47. 2008SERVER+sql2008 r2 装9.4.124提示"SQL SERVEVR 不存在或者拒绝访问"
关闭防火墙并且在sqlserver配置管理器开启命名管道(Named Pipes, TCP/IP)、IP管道。

![](./1.3.47.png) 

[其他可参考1.2节](01.2.md) 

## 48. 问题：es自带视图里有字段A（附件格式）,提公式提取该字段A，写提数公式时，报如下错误。

![](./1.3.48.png) 

解决办法:把当前表的字段改为文字类型，写提数公式，再把文字类型改成附件类型。  
> 其实也可以用转文字函数，即提数时使用：转文字(附件字段)
> 额外提示：同理可应用于图片等非文本型字段对比。
 
## 49. 问题：es自带下拉列表有一个空白，可以跳过必填验证，如何防止？
> @傻子：用code<>63可以控制

![](./1.3.49.png) 
 
## 50. 问题：使用Express版数据库出现备份失败
> @芈悟：权限问题，更改sql运行身份

![](./1.3.50.png) 
 
## 51. 错误：7127
![](./1.3.51.png) 

> @芈悟：c盘：Documents and settings \ administrator\Local Settings \Temp\cesoft
清除该目录下的临时文件，这些文件是填报，修改，打开等产生的临时文件，太多可能会导致该问题。
 
## 52. 错误：9
![](.//1.3.52.png) 

> @芈悟：模板上的表样和数据表的明细表范围不一致导致。

解决：调整为一致

> @潙伱/aiq變乖：【模板属性】-【填报设置】-【保存本地副本时去除工作表保护】取消勾选。
 
## 53. 错误：1004
使用自定义打印，选择打印表头时，出现如下图

![](./1.3.53.jpg) 

> @芈悟：esfunc或者esfunc10未加载。

这个文件在安装目录下可以找到，如果想使用esf_函数，或者自定应打印功能等，都需要加载该宏。

解决：可以手动从安装目录下找到，加载上即可。
 
## 54. exec script, ESSystem
错误：执行脚本文件 [C:\DOCUME~1\ADMINI~1\LOCALS~1\Temp\7zS34.tmp\up_AutoTask.sql] 出错，提交数据库操作出错。违反了 PRIMARY KEY 约束 'PK__ES_BkPlan__3E52440B'。不能在对象 'ES_BkPlan' 中插入重复键。(-2147218877)](-2147181102)

> @芈悟：把安装路径下的esupgtool（老版本升级工具）解压开,找到对应的sql脚本，此处为：up_AutoTask.sql。然后在查询分析器中，根据提示在essystem库下执行该脚本即可。

PS:其他类似提示，也是这种处理方式。esupgtool.exe里面是所有的升级脚本
 
## 55. 错误代码199612
描述：在我的工作台，新建模版的时候提示：office检测到此文件存在一个问题。要保护您的计算机。。。

> @芈悟：office本身的安全设置问题

解决：把下面的文件复制记事本里
```cmd 
Windows Registry Editor Version 5.00 
[HKEY_CURRENT_USER\Software\Microsoft\Office\14.0\Excel\Security\FileValidation]
"EnableOnLoad"=dword:00000000 
```
然后另存为，关闭.reg ，然后双击导入下注册表就关闭文件验证了。
 
## 56. 连接不成功，无法打开登陆所请求的数据库“Essystem”。登陆失败
![](./1.3.56.png) 

原因：该问题可能是账户密码更改或过期。

> @芈悟：重设置SQL的密码后正常。
 
## 57. 错误代码91
![](./1.3.57.png) 

**问题现象：**

填报、查看填报内容、办理待办时报此错误；确定后，模版能打开，但是内容不显示，填报、办理时待办时，点保存无响应。

> @张PM：解决办法如下：

1.看es安装目录，是不是安装到根目录下了。

> @飘泊流浪：卸载PPT美化大师(wps插件)

2.注册一下dll
```sh
	regsvr32 /s msdtcprx.dll
	regsvr32 /s CoESDBAdapter.dll
	regsvr32 /s scrrun.dll
	regsvr32 /s msstdfmt.dll
	regsvr32 /s msxml.dll
	regsvr32 /s msxml2.dll
	regsvr32 /s msxml6.dll
	regsvr32 /s msxml3.dll 
```

3.重装原版系统。

雨木林风的有一个win7版本的系统，无论怎么改，都是报91错误的。 

## 58. com加载项显示esclient10已卸载
在win10\office2016环境,如果用自解压的管理员身份安装335，普通用户登录可能会出现闪退，但是加载项的禁用里没有禁用项目，在com加载项显示esclient10已卸载。解决方案，将自解压文件用压缩文件解压后，用普通用户身份进行安装。

## 59. 错误代码196734
![](./1.3.59.jpg) 

> @绿箭 如果登陆的时候出现以上错误提示，是因为安装文件缺少了.dll文件，具体哪个不好说。

* 原因是360或者其他杀毒软件误杀了勤哲的文件。  
* 解决：打开360软件找到隔离区，恢复.dll文件并且设为白名单即可  
* 彻底解决：删除360软件或者其他杀毒软件
	
## 60. 引用外部数据库视图 ，表间公式提取到 es里面很卡
![](./1.3.60.png) 

> @HKP: 直接sql命令 却很流畅，证明不是sql的问题，经过多方查找 ，原来只要点这里， 就可以瞬间填写完毕，一点也不卡了。遇到类似问题的筒子们 可以参考下。
		
## 61. 错误代码40402，XXXX插入重复键
这个一般是主键重复检查验证失败，默认提示非常晦涩难懂。
![](./1.3.61-1.png) 

> @XY, @昆明-haotian: 可以进sql修改该表的索引名称，使提示便于理解，修改后效果如下

![](./1.3.61-2.jpg) 
![](./1.3.61-3.png) 

> @村长：也可以用触发器来检查重复，错误描述自定义

```
alter trigger t1
on t22  --要设置检查的表
instead of insert
as
    if exists(select * from t22, inserted where t22.rc = inserted.rc) --rc改成要检查的字段
    begin
		raiserror 500002 'Xxxx已存在，数据重复'
        rollback transaction --回滚，避免加入 
    end
```

![](./1.3.61-4.png) 

## 62. ES中的"常量.空值"和"无值"的用法
> @王达

	"字段名=常量.空值" 相当于sql中的 "字段名=null "
	"字段名 无值" 相当于sql中的 "字段名 is null "

	所以提数公式中用空值作为筛选条件时,应写为 "字段名 无值"
	回写公式中需要把某字段改为null,应写为 "字段名=常量.空值"

详细解释可百度[sql三值逻辑](https://www.baidu.com/s?wd=sql三值逻辑)

## 63. 模板的vba代码无法存储
> @王达：是兼容性的问题.

	解决方案:第一次保存时,取消"兼容2003及以下版本"的勾再点保存,
	之后设计状态下再打开,重新勾上"2003",再点继续即可
	
## 64. 关于 X 哲提示版本不一致问题处理
![](./1.3.64-1.jpg) 

> @Mr.Qili：ESSystem.ES_SysInfo 中 Version 值被清空，数据库被写了加密的触发器，删除后，填上版本号，系统正常。

![](./1.3.64-2.jpg)

> @村长补记，[3.2以毒攻毒法](https://esbook.erp8.net/c3/03.2.html)

> @傻木 - 检查库中所有触发器：
```
SELECT object_name(a.parent_obj) as [表名]  
    ,a.name as [触发器名称]  
    ,(case when b.is_disabled=0 then '启用' else '禁用' end) as [状态]  
    ,b.create_date as [创建日期]  
    ,b.modify_date as [修改日期]  
    ,c.text as [触发器语句]  
FROM sysobjects a  
    INNER JOIN sys.triggers b  
        ON b.object_id=a.id  
    INNER JOIN syscomments c  
        ON c.id=a.id  
WHERE a.xtype='tr' 
ORDER BY [表名]
```
通常不应该存在触发器，除了tWxtx^_^。
	
## 65. ES用户被清空

> @王达：今天有人说他的es_users表总是莫名其妙的被清空。我远程看了下，发现是getnewid_s存储过程被人改了，里面加了句delete from es_user表。我删掉那句后就一切正常了
	
## 66. 错误2147220372：用户被锁定
![](./err2147220372.png)

> @平淡人生：ES_User表AccState字段，把ADMIN这一行的值改为0

## 99. 新手BUG
|问题 |解决方案|
|:----|:----|
|插入行后Excel公式不自动扩展 |检查字段是否有链接|
|不能网页填报 |1模板属性WEB填报勾选2设计模板保存时，勾上WEB填报|
|无法启动服务|1杀毒误杀 2硬件软件环境发生变化 重新注册|
|不执行表间公式|工作流中执行人是否给予执行权限（管理模板“操作”打勾）|
|错误代码1004|提取公式错误，设计思路问题|
|错误代码10055|服务器地址改192.168.0.×××|
|错误代码10061|服务器端口未打开或者被占用|
|错误代码19772|重启电脑|
|错误代码40101|可能更改了计算机名，需要在系统设置中更改数据库服务器名称|
|错误代码40402|填报违反设计规则：如主键|
|错误代码40411|去数据库删除些日志或者调大日志空间(未验证)|
|错误代码-24605|ESWint10.dll 未替换 端口未开 360 等等情况|
|错误代码-2147181102|1安装数据库不通  2表单权限乱|
|错误代码429|360拦截或重装客户端|
|错误代码43700|服务器7779端口没打开|
|错误代码91|单元格格式 不对，或者与已有表单格式冲突（待测试）|
|登陆记录删除|安装地址找 logininfo.xml 修改内容|
|公式数字无法改动|模板属性 填报设置 保护时，锁定EXCEL公式 勾弃掉|
|回写日期为空|辅助单元格为'1900/1/1与显示单元格关联为""|
|回写新建表单|无值用<>|
|模板无法打开|关闭模板设计，非法关闭造成重启|
|权限问题|勾掉查阅即可 查看待办事宜|
|闪退（"EXCEL服务器菜"单丢失）|excel禁用加截项，启用|
|数字不全|EXCEL设定文本 勤哲也需要设定文本格式，数值 勤哲设定对应小数整数金额|
|锁定单元格|提数公式内有锁定，或整表锁定，需给予锁定权限，通过回写可以修改锁定表单|
|提数公式后，数据保护|表间公式第三步 填充项中的勾弃掉，（单元格不能有公式）|
|无法导出|导出表单会跳出EXCEL空文件 不能关闭 否则无法导出|
|无法导入|首行排序筛选 删除|
|无法删除字段|必填项、主键项、工作流都需删除（设计模板-菜单栏有“引用”查询）|
|无法提取数字|单元格设置“常规”，提取的数字“文本”格式会暗藏单引号|
|许多无法判定的功能|加辅助字段|
|主键不起作用|是不是勾选2个主键，变成联合主键不起作用|
|设计模板OK，填报公式不执行|注意'1900文本格式和1900数字格式对比（上引号会被隐藏）|
|只显示本人填报|高级查阅中勾选本人填报|
|日期如何带时间|文本格式 带时间。日期格式 设置仅仅日期|
|decimail|我会告诉你是自动新建邮件不能用数字回写么|
|新建没有的供应商|供应商名称 不属于 此集合(供应商名称 )| 
|工作台某条数据查看时勤哲关闭退出|在数据库把这记录删了，重做，又好了？| 
|明细只有一条数据，设必填后就保存不了|EXCEL公式就加个IF排空，例如if(B4="","",原公式)| 

### 本节贡献者
*@张PM*  
*@kangroo*  
*@毛毛*  
*@不笨的猪♂*  
*@新新*  
*@adobe大仙*  
*@Meteor*  
*@傻木*  
*@XY*  
*@-196度*  
*@芈悟*  
*@ゝ莫说*  
*@路柏*  
*@飘泊流浪*  
*@旺旺lc801024*  
*@范味浓*  
*@在路上*  
*@绿箭*  
*@简单生活*  
*@HKP*  
*@王达*  
*@Mr.Qili*  
*@潙伱/aiq變乖*  
