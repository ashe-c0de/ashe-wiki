## 背景

https://www.elastic.co/guide/en/elasticsearch/client/java-api-client/current/installation.html

以上是Elasticsearch对接Java的官方文档（pom依赖部分）
![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/sb-es-1.png)

我本地Windows安装的Elasticsearch也是8.15.3版本
![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/sb-es-2.png)

## 启动报错
```java
/**
 * Java Client: 8.10
 * <a href="https://www.elastic.co/guide/en/elasticsearch/client/java-api-client/current/connecting.html">api reference</a>
 */
@Configuration
public class ElasticsearchConf {

    String fingerprint = "28cd510c461b5d1e0c6c3e47d5b85a72eb8a61a1de2164d493b83ee901465ee9";
    String username = "elastic";
    String password = "C3BYD*FS14c6l=a+TpgR";

    /**
     * Create the low-level client
     */
    @Bean
    public RestClient restClient() {
        SSLContext sslContext = TransportUtils
                .sslContextFromCaFingerprint(fingerprint);
        BasicCredentialsProvider credsProv = new BasicCredentialsProvider();
        credsProv.setCredentials(
                AuthScope.ANY, new UsernamePasswordCredentials(username, password)
        );

        // 如果是多节点的集群环境
//        List<String> hosts = Arrays.asList("https://localhost:9200","https://localhost:9201","https://localhost:9203");
//        HttpHost[] array = hosts.stream().map(HttpHost::create).toArray(HttpHost[]::new);
//        RestClient restClient = RestClient.builder(array)
//                .setHttpClientConfigCallback(hc -> hc
//                        .setSSLContext(sslContext)
//                        .setDefaultCredentialsProvider(credsProv)
//                )
//                .build();

        return RestClient
                .builder(new HttpHost("localhost", 9200, "https"))
                .setHttpClientConfigCallback(hc -> hc
                        .setSSLContext(sslContext)
                        .setDefaultCredentialsProvider(credsProv)
                )
                .build();
    }

    /**
     * Create the transport with a Jackson mapper
     */
    @Bean
    public ElasticsearchTransport transport() {
        return new RestClientTransport(
                restClient(), new JacksonJsonpMapper());
    }

    /**
     * Synchronous blocking client 同步阻塞式client
     */
    @Bean
    public ElasticsearchClient syncClient() {
        return new ElasticsearchClient(transport());
    }

    /**
     * Asynchronous non-blocking client 异步非阻塞式client
     */
    @Bean
    public ElasticsearchAsyncClient asyncClient() {
        return new ElasticsearchAsyncClient(transport());
    }

}
```

```bash
***************************
APPLICATION FAILED TO START
***************************

Description:

An attempt was made to call a method that does not exist. The attempt was made from the following location:

    co.elastic.clients.transport.rest_client.SafeResponseConsumer.<clinit>(SafeResponseConsumer.java:52)

The following method did not exist:

    org.elasticsearch.client.RequestOptions$Builder.setHttpAsyncResponseConsumerFactory(Lorg/elasticsearch/client/HttpAsyncResponseConsumerFactory;)Lorg/elasticsearch/client/RequestOptions$Builder;

The method's class, org.elasticsearch.client.RequestOptions$Builder, is available from the following locations:

    jar:file:/C:/repository/java/org/elasticsearch/client/elasticsearch-rest-client/6.4.3/elasticsearch-rest-client-6.4.3.jar!/org/elasticsearch/client/RequestOptions$Builder.class

It was loaded from the following location:

    file:/C:/repository/java/org/elasticsearch/client/elasticsearch-rest-client/6.4.3/elasticsearch-rest-client-6.4.3.jar


Action:

Correct the classpath of your application so that it contains a single, compatible version of org.elasticsearch.client.RequestOptions$Builder
```
报错翻译：大意是指正在调用一个不存在的method，这个method对应的class在我的本地maven仓库的xxx路径，并提到了elasticsearch-rest-client的版本，给出的建议是纠正该client的唯一性&兼容性……

我Google了一下报错内容，在Elasticsearch官网中找到了一个类似的issue，官方给出的回答是elasticsearch-rest-client版本不对，但是那个issue最后并未被发起者确认解决问题，而是在一段时间后自动关闭了。

## 依赖分析
![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/sb-es-3.png)
发现官方推荐的依赖中集成的elasticsearch-rest-client版本是6.4.3而非8.15.3

因此尝试手动添加elasticsearch-rest-client8.15.3版本的依赖
```bash
        <dependency>
            <groupId>org.elasticsearch.client</groupId>
            <artifactId>elasticsearch-rest-client</artifactId>
            <version>8.15.3</version>
        </dependency>
```

再次启动项目，启动成功。

> 起初我以为是阿里镜像中上传的elasticsearch-java依赖有误，在调试过程中，我尝试把maven的settings.xml文件中的阿里镜像替换成Apache仓库，发现仍然存在该问题，因此可以确定应该是官方制作依赖时留下的bug？


后记：

在8.16.0 version中，pom依赖仍然需要额外引入elasticsearch-rest-client
```bash
        <dependency>
            <groupId>co.elastic.clients</groupId>
            <artifactId>elasticsearch-java</artifactId>
            <version>8.16.0</version>
        </dependency>

        <dependency>
            <groupId>org.elasticsearch.client</groupId>
            <artifactId>elasticsearch-rest-client</artifactId>
            <version>8.16.0</version>
        </dependency>
```

并且在使用过程中发生报错
```bash
java.io.IOException: Host name '${server_ip}' does not match the certificate subject provided by the peer (CN=${instance_name})
```

我检索了该报错，在Elasticsearch官网有相关issue页面

https://discuss.elastic.co/t/host-name-does-not-match-the-certificate/186618

![p](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/sb-es-4.png)

初始化RestClient的时候，需要配置跳过SSL证书验证（8.15.3 version则不需要配置该项）
```java
.setSSLHostnameVerifier(NoopHostnameVerifier.INSTANCE)
```