# CMPP 配置

## 案例-保互通

根据通道商给的账号信息在网关服务器上配置网关程序配置文件 `SmsGateway.ini`。

```ini
[System]
Title = 金慧盈通短信服务网关(君诚科技保互通)
;--- 通道值: 460102,460322,...
Operator = 460204
;--- 网关的 IP\Port,针对电信收模式端口\用户名\密码
ServerIP = 120.25.132.170
ServerPort = 7890
LoginName = 301026
LoginPwd  = password
;--- SP 接入号(显示在用户手机上的号码)\计费号码\ SP 企业代码
SpNumber = 1069081777688
SpID = 301026
;--- 通道值
Operator = 460204
```

### 老平台配置

* 老平台在数据库 `KMC31_SMSG` 的 `TBL_SMS_OPERATORPARA` 和 `TBL_SMS_OPERATOR` 两个表新增一条通道，通道号要和配置文件上配置的一样。

```sql
insert into KMC31_SMSG..TBL_SMS_OPERATORPARA
(sms_operator,sms_priority,sms_priorityd,gate_status,gate_para1,gate_para2,createtime)
values('460204','9','9',1,'60','1',getdate())

insert into TBL_SMS_OPERATOR(SMS_OPERATOR,TELE_NAME,SMS_SPPORT,SMS_KIND,SMS_OPERATORKIND,SMS_SPEED,SMS_RESEND,AREA_ID)
VALUES('460204','君诚科技_保互通','1069081777688',1,7,300,300,100000000)
```

**修改**数据库 `KMC31_SMSG` 存储过程 `usp_sms_MessageTempYz`，把新通道号 460204 加进去。

```sql
update tb1_sms_sndtmp set sms_operator=460777 where snd_id in (
select snd_id from tb1_sms_sndtmp as a
where a.sms_operator not in (460103,460310,460188......460204))
```

* 把 `SmsGateway.exe` 重命名为 `SmsGateway-jckjbht-460204.exe` 并创建快捷方式放在桌面 boot 文件夹里，启动 `SmsGateway-jckjkftz-460204.exe`，查看有无报错。

### 新平台配置

* 登录 web 短信新平台，新增一条通道，点击“通道管理”->“通道设置”->“增加”，通道号要和配置文件上配置的一样。
* 把 `SmsGateway.exe` 重命名为 `SmsGateway-jckjkftz-460205.exe` 并创建快捷方式放在桌面 boot 文件夹里，启动 `SmsGateway-jckjkftz-460205.exe`，查看有无报错。

### 测试短信

* 登录 web 短信平台，使用电信、移动、联通号码测试发送并验证短信是否正常收到；如果有异常，可以把相应返回码反馈给通道商寻求技术支持。
