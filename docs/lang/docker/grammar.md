# Docker command

> reference: https://docs.docker.com/get-started/overview/

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/lang/docker.png)

## Images

An image is a read-only template with instructions for creating a Docker container. // image是一个只读模板，包含创建 Docker container 的说明。

无论是Java程序还是C++或其他程序，都可以按照Dockerfile规范统一打包成Image，然后由Docker创建container运行。

### Containers

A container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI.

## Images

As you see, we need pull image to our Docker Host (Docker宿主机，即你使用的computer) from Docker Hub, then use command create a container by client.

```bash
docker pull nginx

docker images  # you can see nginx's image
```

You can also create an image by Dockerfile, like this:

> upload your-app.jar to current folder
> vim Dockerfile

```dockerfile
FROM openjdk:17-jdk

WORKDIR /app

# 将当前目录下的jar复制到Docker容器中的指定目录下
COPY ./*.jar /app/your-app.jar

# 容器启动时执行的命令
CMD ["java", "-jar", "/app/your-app.jar"]
```

Save & Exit

```bash
docker build -t you-app .  # then Docker will create an image by your-app & Dockerfile of the current folder
docker images  # you can see you-app's image
```

## Containers

```bash
运行容器：docker run -it --name my-nginx -p 80:80 nginx  # -it 以交互模式启动容器并返回终端

列出所有的容器：docker ps -a

退出容器：exit / ctrl + p + q (退出，但不shutdown)

重启容器：docker restart image_id/name

停止容器：docker stop image_id/name

注销容器：docker kill image_id/name

删除已停止容器：docker rm image_id

守护线程启动容器：docker run -d name  # Docker容器后台运行，不阻塞控制台的继续使用

复制文件到容器指定路径：docker cp index.html 652483e70a9f:/usr/share/nginx/html/index.html

将修改的容器提交为新镜像：docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

将镜像保存为.tar格式文件：docker save [OPTIONS] IMAGE [IMAGE...]

加载.tar格式文件为镜像：docker load [OPTIONS]

查看容器日志：docker logs -f container_id
```
