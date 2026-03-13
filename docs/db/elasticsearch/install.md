安装Elasticsearch__基于Windows系统

https://www.elastic.co/downloads/elasticsearch

1、先下载压缩包，然后解压

~~2、修改配置文件的字符集~~
![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/es-conf.png)
在此文件末尾添加一行（此步骤仅限于8.15.3 version或之前的版本，之后我安装8.16.0version则不需要修改此配置文件）
`-Dfile.encoding=GBK`

3、启动Elasticsearch
在解压目录下，打开terminal，键入以下指令

`bin\elasticsearch.bat`
![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/es-password.png)
在第一次启动的时候，会自动配置好elastic用户的密码并输出至terminal中（如果你没有修改配置文件的字符集，那么这部分输出可能会乱码）

4、浏览器访问验证（需要输入账户密码，账户为elastic，密码则是上面自动配置生成的内容）
https://localhost:9200/

![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/es-host.png)