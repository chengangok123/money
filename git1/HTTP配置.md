# HTTP 配置

**注意**：通道配置好后，测试前，需将此账号的报告地址与回执地址报给君诚科技录入

## 案例-君诚科技-东莞证券

根据通道商给的账号信息在 120.76.188.194 上配置网关程序配置文件 `config.xml`

```ini
<operators>
<item>460199</item>
</operators>
...
operation>
<rule>460199</rule>
</operation>
...
<report>
<rule>460199</rule>
</report>
...
<smipool>
    <smi>
<localport>6102</localport>
<loginname>jhyt_dgzq</loginname>
<password>password</password>
<operatorid>460199</operatorid>
    </smi>
</smipool>
```

* 该程序通道号为 460199（此通道为我司定义）
* 该程序所监控的端口号为 6102（我司定义）
* 用户名为 jhyt\_dgzq（通道商提供）
* 密码（通道商提供）

### 老平台配置

* 老平台在数据库 `KMC31_SMSG` 的 `TBL_SMS_OPERATORPARA` 和 `TBL_SMS_OPERATOR` 两个表新增一条通道，通道号要和配置文件上配置的一样。

```sql
INSERT INTO KMC31_SMSG..TBL_SMS_OPERATORPARA
(SMS_OPERATOR,SMS_PRIORITY,SMS_PRIORITYD,GATE_STATUS,GATE_PARA1,GATE_PARA2,CREATETIME)
VALUES('460199','9','9',1,'60','1',GETDATE())

INSERT INTO TBL_SMS_OPERATOR(SMS_OPERATOR,TELE_NAME,SMS_SPPORT,SMS_KIND,SMS_OPERATORKIND,SMS_SPEED,SMS_RESEND,AREA_ID)
VALUES('460199','君诚科技_东莞证券',xxxxxx,1,7,300,300,100000000)
```

* 修改数据库 `KMC31_SMSG` 存储过程 `usp_sms_MessageTempYz`，把新通道号 460199 加进去。

```sql
update tb1_sms_sndtmp set sms_operator=460777 where snd_id in (
select snd_id from tb1_sms_sndtmp as a
where a.sms_operator not in (460103,460310,460188......460199))
```

* 把 exe 程序重命名为 `jckjdgzq-460199.exe` 并创建快捷方式放在桌面 boot 文件夹里，启动 `jckjdgzq-460199.exe`，查看有无报错。

### 新平台配置

* 登录[短信新平台](http://sms.kdsapp.com:8080/sms/)，新增一条通道，点击“通道管理”->“通道设置”->“增加”，通道号要和配置文件上配置的一样。
* 把 exe 程序重命名为 `jckjdgzq-460199.exe` 并创建快捷方式放在桌面 boot 文件夹里，启动 `jckjdgzq-460199.exe`，查看有无报错。

### 测试短信
