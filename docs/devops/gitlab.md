选择使用清华大学的镜像站来下载社区版的GitLab
https://mirror.tuna.tsinghua.edu.cn/help/gitlab-ce/

`sudo vim /etc/yum.repos.d/gitlab-ce.repo`
```bash
[gitlab-ce]
name=Gitlab CE Repository
baseurl=https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el$releasever/
gpgcheck=0
enabled=1
```
```bash
yum makecache
```

查看可用版本
```bash
yum list gitlab-ce --showduplicates
```

安装指定版本（不指定版本时默认最新版本 yum install gitlab-ce）
```bash
yum install gitlab-ce-11.1.1
```

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/gitlab.png)

于/etc/gitlab/gitlab.rb配置文件处，修改你的GitLab外部访问地址

默认GitLab外部访问地址是：external_url 'http://gitlab.example.com'

需要将其改成访问你服务器的GitLab地址，由于我没有域名只有ip，我的修改为`external_url 'http://server_ip:10086'`

修改后刷新配置`gitlab-ctl reconfigure`

重新启动`gitlab-ctl restart`

在操作系统层面的防火墙放行你配置访问本地GitLab的端口

`sudo firewall-cmd --add-port=10086/tcp --permanent `

`sudo firewall-cmd --reload`

`sudo firewall-cmd --list-all`

由于我的服务器是阿里云的云服务器ECS，因此还需要在安全组规则添加网络层面的端口放行。

查看GitLab应用root用户生成的默认密码

`cat /etc/gitlab/initial_root_password`

做好密码备份，该文件会在24小时后自动删除，如果你想修改root的密码，请参考https://docs.gitlab.com/ee/security/reset_user_password.html

![](https://raw.githubusercontent.com/ashe-c0de/wiki-src/refs/heads/main/db/gitlab-login.png)

使用浏览器访问`server_ip:10086`，键入 用户 & 密码 登录即可。

停止Gitlab程序进程 `gitlab-ctl stop`