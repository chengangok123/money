# SGIP 配置

## 案例（盈通-MAS-联通）

根据运营商给的账号信息在网关服务器上配置网关程序配置文件 `SmsGateway.ini`

```ini
;短信发送通道:Operator1~n
[Operator1]
;---协议类型0:联通短信网关,1:电信网关,2:移动网关\通道名称
ProtocolType = 0
Name = MAS-联通

;---工作模式:针对电信网关 0,1:收模式和发模式分离 2:收发模式
LOGIN_MOD = 2
;---针对电信网关 收模式和发模式分离中，获取状态报告的间隔时间，单位:秒
RecvReportTime = 5

;---网关的IP\Port\针对电信收模式端口\用户名\密码
ServerIP = 123.125.99.61
ServerPort = 5003
TelnetRecvPort =
LoginName = 000201
LoginPwd  = 密码

;---针对联通网关 网关IP\本地侦听端口Port\用户名及密码\最大连接数
ListenIP = 123.125.99.60
ListenPort = 6108
ListenName =
ListenPwd  =
MaxConn = 5

;---SP接入号(显示在用户手机上的号码)\计费号码\SP企业代码
SpNumber = 10692639
ChargeNumber =
SpID = 87045
ServiceId =

;---针对联通网关 节点ID
NodeNumber = 3000089692
```

### 老平台配置

老平台在数据库 `KMC31_SMSG` 的 `TBL_SMS_OPERATORPARA` 和 `TBL_SMS_OPERATOR` 两个表新增一条通道，通道号要和配置文件上配置的一样

```sql
INSERT  INTO KMC31_SMSG..TBL_SMS_OPERATORPARA
        ( sms_operator ,
          sms_priority ,
          sms_priorityd ,
          gate_status ,
          gate_para1 ,
          gate_para2 ,
          createtime
        )
VALUES  ( '460338' ,
          '9' ,
          '9' ,
          1 ,
          '60' ,
          '1' ,
          GETDATE()
        )

INSERT  INTO TBL_SMS_OPERATOR
        ( SMS_OPERATOR ,
          TELE_NAME ,
          SMS_SPPORT ,
          SMS_KIND ,
          SMS_OPERATORKIND ,
          SMS_SPEED ,
          SMS_RESEND ,
          AREA_ID
        )
VALUES  ( '460338' ,
          '盈通-MAS-联通' ,
          '10692639' ,
          1 ,
          7 ,
          300 ,
          300 ,
          100000000
        )
```

修改数据库 `KMC31_SMSG` 存储过程 `usp_sms_MessageTempYz`，把新通道号 460338 加进去

```sql
UPDATE  tb1_sms_sndtmp
SET     sms_operator = 460777
WHERE   snd_id IN (
        SELECT  snd_id
        FROM    tb1_sms_sndtmp AS a
        WHERE   a.sms_operator NOT IN ( 460103, 460310, 460188,..., 460338 ) )
```

把 `SmsGateway.exe` 重命名为 `SmsGateway-jhyt-mas-lt-460338.exe` 并创建快捷方式放在桌面 boot 文件夹里，启动程序查看有无报错

### 新平台配置

登录 web 短信新平台，新增一条通道，点击“通道管理”->“通道设置”->“增加”，通道号要和配置文件上配置的一样，把 `SmsGateway.exe` 重命名为 `SmsGateway-jhyt-mas-lt-460338.exe` 并创建快捷方式放在桌面 boot 文件夹里，启动程序查看有无报错

### 测试短信

登录 web 短信平台，使用运营商对应号段测试发送并验证短信是否正常收到；如果有异常，可以把相应返回码反馈给运营商寻求技术支持
