> 在Elasticsearch中创建好index（可以理解为关系型数据库的table），然后在Kibana页面创建index对应的data-veiw（document的可视化界面）。

> 整个ELK流程简而言之，就是springboot输出日志到服务器某个指定文件上，然后Logstash程序通过读取这个日志文件，并把日志插入到Elasticsearch的index中，然后你就可以通过Kibana的data-view页面检索你想要查询的日志信息。

## 安装Logstash

https://www.elastic.co/guide/en/logstash/8.16/installing-logstash.html

尽量保持你的Elasticsearch、Kibana、Logstash的version一致，我所使用的version为8.16.0

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/elk-1.png)

启动成功，并演示了hello world后，就可以准备Logstash的配置文件和SpringBoot的jar了。

## 准备SpringBoot的jar

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/elk-2.png)

log4j2-spring.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="INFO">
    <Appenders>
        <!-- 日志文件输出 日志 -->
        <RollingFile name="File" fileName="logs/my_app.log"
                     filePattern="logs/$${date:yyyy-MM}/my_app-%d{MM-dd-yyyy}-%i.log.gz">
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
            <Policies>
                <!-- 指定的时间间隔触发日志滚动，未配置，默认1小时 -->
                <TimeBasedTriggeringPolicy/>
                <!-- 日志文件大小达到指定大小时触发日志滚动，Log4j2 就会滚动日志文件 -->
                <SizeBasedTriggeringPolicy size="10MB"/>
                <!-- 满足其中一个触发条件（时间/文件大小），都会执行日志滚动 -->
            </Policies>
            <!-- 最多保留 10 个滚动的日志文件，有限删除最旧的日志文件 -->
            <DefaultRolloverStrategy max="10"/>
        </RollingFile>
        <!-- 控制台输出 日志 -->
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
    </Appenders>

    <Loggers>
        <Root level="info">
            <AppenderRef ref="File"/>
        </Root>
    </Loggers>
</Configuration>
```

非常简单的一个项目，通过请求接口的参数控制日志内容

## 准备Logstash的配置文件

```bash
input {
  file {
    path => "/home/es/temp/logs/my_app.log" # SpringBoot日志的绝对路径
    start_position => "beginning"
    sincedb_path => "/dev/null"
  }
}

filter {
  # 可以在这里添加过滤器来处理日志，例如使用grok解析日志格式
}

output {
  elasticsearch {
    hosts => ["https://your.elasticsearch.ip:9200"]
    index => "cat_index" # 你创建的index的name
    user => "elastic"
    password => "your_password"
    ssl => true
    ssl_certificate_verification => false # 禁用ssl验证
  }
}
```

如果你的Elasticsearch本身是http访问，那么可以忽略这个配置）关于禁用ssl验证那里，也可以将Elasticsearch服务器的CA证书（通常是.crt文件）导入到Java的信任库中。 我没尝试那种做法~

通过这个配置文件来启动Logstash （我将其保存在了Logstash的conf目录下）
`bin/logstash -f /home/es/logstash-8.16.0/config/logstash.conf`

## data-view页面验证日志是否成功插入

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/elk-3.png)
![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/elk-4.png)

通过调用接口，less日志文件，可以看到已经有对应的日志输出了。

这时我们在data-view的检索窗口，尝试检索“Expressing”

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/elk-5.png)

可以看到，已经查询到了对应日志所映射生成的document了。

在Kibana页面还可以更方面的展开日志

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/elk-6.png)