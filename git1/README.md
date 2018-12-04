# 网关服务

HTTP 和 CMPP 网关都调用以下四个存储过程，存储过程名称相同，功能实现步骤有所差异

* USP\_SMG_LoadOperSendMessage：根据传参更新短信发送表状态、加载（查询）发送情况
* USP\_SMG_SaveRecvMessage：调用存储过程插入短信上行表
* USP\_SMG_UpdateMessageReportStatus：传参更新短信历史表状态值
* USP\_SMG_UpdateMessageStatus：建立当月历史表，并将发送表 tmp 的数据转移到发送历史表中
