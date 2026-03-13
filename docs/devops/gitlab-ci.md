## 准备工作

服务器配置：2c8g，最好大于等于8g否则可能内存不足

系统环境：CentOS Linux release 7.9.2009 (Core)

安装包：gitlab-ce-11.1.1-ce.0.el7.x86_64.rpm 和 gitlab-runner-11.1.1-1.x86_64.rpm

 

CI/CD流程：

- 代码推送： 开发者将代码推送到GitLab的远程仓库。
- 触发CI/CD Pipeline： 当代码被推送到GitLab仓库时，GitLab会检测到仓库的变化，并根据仓库根目录下的.gitlab-ci.yml文件配置来触发CI/CD Pipeline。
- GitLab Runner： GitLab实例会将构建任务发送给注册的GitLab Runner。GitLab Runner执行job并将结果发送回GitLab。
- 执行Job： GitLab Runner会克隆仓库的代码到其工作目录，并开始执行.gitlab-ci.yml文件中定义的各个job。每个job是一个构建脚本，可以执行各种任务，比如编译代码、运行测试、部署应用到服务器等。

## 操作流程：

### 下载并安装

由于网络等原因，我选择清华大学镜像站下载安装包至本地（通过配置/etc/yum.repos.d/gitlab-ce.repo的方式，以yum在线安装也可以，但是遇到问题，卸载重新安装又要再次等待网络下载的时间，比较麻烦）
```bash
wget --user-agent="Mozilla" https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/gitlab-ce-11.1.1-ce.0.el7.x86_64.rpm
```

```bash
wget --user-agent="Mozilla" https://mirrors.tuna.tsinghua.edu.cn/gitlab-runner/yum/el7/gitlab-runner-11.1.1-1.x86_64.rpm
```

rpm命令安装
```bash
rpm -ivh gitlab-ce-11.1.1-ce.0.el7.x86_64.rpm
```
```bash
rpm -ivh gitlab-runner-11.1.1-1.x86_64.rpm
```

### 配置

GitLab就需要修改下外部访问GitLab地址
```bash
vim /etc/gitlab/gitlab.rb
```

默认GitLab外部访问地址是：external_url 'http://gitlab.example.com'

需要将其改成访问你服务器的GitLab地址，由于我没有域名只有ip，我的修改为external_url 'http://server_ip:10086'

修改后刷新配置gitlab-ctl reconfigure

重新启动gitlab-ctl restart

在操作系统防火墙放行你GitLab的端口（如果是云服务器，还需要在网络层面放行端口）

sudo firewall-cmd --add-port=10086/tcp --permanent

sudo firewall-cmd --reload

sudo firewall-cmd --list-all

 

然后浏览器访问http://server_ip:10086

页面会让你初始化密码，用户默认为root。

然后通过root账户登录，创建Group和Project，进入project的设置项。

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/devops/gitlab-ci-1.png)

在这里，获取到GitLab Runner需要使用的token，GitLab Runner通过它注册至GitLab实例。

配置GitLab Runner，这里有一个坑，GitLab Runner本身是以gitlab-runner这个用户在执行指令。

因此gitlab-runner start直接启动GitLab Runner会发现启动不起来（gitlab-runner status），通过sudo journalctl -u gitlab-runner -f可以查看启动时的相关日志，其中会提及到没有权限创建/home/gitlab-runner这个目录（这是其默认的工作目录）。

这是因为GitLab Runner已经安装完毕，通过cat /etc/passwd可以看到gitlab-runner这个用户，所以GitLab Runner的行为实际上是以gitlab-runner用户来执行的。

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/devops/gitlab-ci-2.png)

由此，我们手动创建其默认的工作目录/home/gitlab-runner，然后将这个工作目录的用户更改为gitlab-runner且允许读写权限

```bash
sudo chown gitlab-runner:gitlab-runner /home/gitlab-runner
```

```bash
sudo chmod 700 /home/gitlab-runner
```

然后启动
```bash
sudo gitlab-runner start
```

```bash
sudo gitlab-runner status
```

就可以看到terminal输出gitlab-runner: Service is running!表示正在运行了。

 

至此，我们可以将GitLab Runner注册至GitLab上了。有两种方式：通过命令（gitlab-runner register）注册；直接编辑GitLab Runner的配置文件（/etc/gitlab-runner/config.toml）来注册。

 

初次建议使用命令注册（然后对比注册前后配置文件发生的变化）

注册成功后，重启Gitlab Runner
```bash
gitlab-runner stop
```
```bash
gitlab-runner start
```
于Gitlab页面查看，Runner是否注册成功且为激活状态

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/devops/gitlab-ci-3.png)

## 推送代码触发pipeline

首先新建一个项目推送至你的Gitlab服务器。

然后在项目根路径下新建一个.gitlab-ci.yml文件。Gitlab服务器会根据这个文件来触发Pipeline，使得Gitlab Runner来执行yml文件中的job。

```bash
---
stages:
  - simple

# 输出 Hello World
# 注意这个tags标签，是和你创建的Gitlab Runner绑定的，如果你的runner不存在这个tag，那么job则不会执行
hello_world:
  stage: simple
  script:
    - echo "Hello World"
  tags:
    - oh
```
推送代码，然后在Pipeline中查看job是否被成功执行。

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/devops/gitlab-ci-4.png)

点进去可以查看具体执行的细节（CI/CD验证成功）

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/devops/gitlab-ci-5.png)


## 将Pipeline的job尝试替换为构建应用并运行(CI/CD)

在此，我的项目为SpringBoot项目，因此分为两个步骤

构建：maven clean package

运行jar包：nohup java -jar xxx.jar

因此修改.gitlab-ci.yml文件

```yml
 stages:
  - build
  - deploy

build_job:
  stage: build
  script:
    - echo "build start"
    - pwd
    - mvn clean package
    - echo "build end"
  artifacts:
    paths:
      - target/  # 将target目录保存为artifacts
  tags:
    - oh

deploy_job:
  stage: deploy
  script:
    - echo "deploy start"
    - pwd
    - ls -l  # 列出当前目录内容，确认是否接收到artifacts
    - cp /home/deploy/start.sh start.sh
    - chmod +x start.sh
    - ./start.sh
    - echo "deploy end"
  dependencies:
    - build_job  # 确保这个job依赖于build_job
  tags:
    - oh
```

上述内容中的启动脚本/home/deploy/start.sh，这个文件依然要确保用户为gitlab-runner否则还是会失败（要明确yml中的所有行为都是由gitlab-runner用户执行的）

推送代码，触发CI/CD，然后查看job的执行情况

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/devops/gitlab-ci-6.png)

在服务器上，通过ps -ef | grep ${程序名}查看进程的启动时间是否与你CI/CD的部署时间吻合，若一致则说明整个构建&部署流程全部成功。

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/devops/gitlab-ci-7.png)

最后，你可以通过修改接口推送代码，测试接口，来判断当前部署的程序是否已经是更新后的程序。
