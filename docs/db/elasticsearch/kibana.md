安装Kibana__基于Windows系统

> 在此之前，请先安装Elasticsearch并启动，并创建一些index

## 一、下载安装包并解压安装
https://www.elastic.co/downloads/kibana
![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/es-k-1.png)

terminal进入Kibana安装目录，通过.\bin\kibana.bat指令启动。
> 如果你是安装在Linux服务器上，需要其他电脑访问kibana页面，那么需要修改conf/kibana.yml文件，将server.host: "localhost"改为server.host: "0.0.0.0"，将允许Kibana监听所有网络接口上的连接，而非本机的连接（默认配置则只允许本机访问kibana页面）

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/es-k-2.png)

## 二、配置后登录Kibana页面
启动成功，访问http://localhost:5601/，页面会要求你填入Enrollment token（此举是将Kibana注册至你的Elasticsearch上）
![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/es-k-3.png)

EnrollmentToken则是在Elasticsearch的bin目录下的elasticsearch-create-enrollment-token.bat脚本生成

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/es-k-4.png)

因此，还是通过terminal执行该脚本获取EnrollmentToken，备份保存，填入Kibana页面后会要求你输入验证码，这个验证码可以在启动Kibana的terminal中找到

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/es-k-5.png)

然后通过Elasticsearch的用户密码登录Kibana

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/es-k-6.png)

## 三、通过Discover快速检索Document

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/es-k-7.png)

第一次进入Discover时，会要求你Create data view，所谓的data view其实就是一个数据查询页面（与index所对应），比如创建了一个products的index，那么你可以按如下方式创建data view

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/es-k-8.png)

然后，你就可以通过这个data view非常方便的快速检索products这个index下的所有document。 

比如我创建了一个users的index，其中插入了一些document，那么我可以按如下方式快速查询：

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/es-k-9.png)

点击检索出来的结果，还可以更直观地查阅document

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/es-k-10.png)

