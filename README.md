# Dockerfiles

用 Docker 容器服务的方式搭建 nginx/php/mysql/redis/go/node 环境，易于维护、升级。

## 配置

你可以通过复制 && 修改 .env.example 来自定义构建项目组合

示例: 
```
cp .env.example .env
vi .env
```

## 使用

进入项目所在目录：
执行命令：
```
docker-compose up
```

如果没问题，下次启动时可以以守护模式启用，所有容器将后台运行：  
```
docker-compose up -d
``` 

使用 docker-compose 基本上就这么简单，Docker 就跑起来了，用 stop，start 关闭开启容器服务。  
更多的是在于编写 dockerfile 和 docker-compose.yml 文件。 

可以这样关闭容器并删除服务：
```
docker-compose down
```

### 3. 测试

将项目源码放到 `~/app` 目录下, 并运行

```
cd src
echo "<?php phpinfo();" > index.php
```

打开 url 访问 `http://localhost/index.php`

### 4. 常用指令

- 帮助
    ```
    docker-compose --help
    ```
- 列出网络 (包括跨群集中多个主机的网络)
    ```
    docker network ls
    ```
- 运行时，指定配置文件  
    ```
    docker-compose -p java -f docker-compose-tomcat.yml up -d
    ```
    **参数**:
    - `-p` 工程名称, 这里为 java, 代表java 相关配置
    - `-f` 配置文件
    - `-d` 后台运行

- 常用`shell`组合

    ```
    # 删除所有容器
    docker stop `docker ps -q -a` | xargs docker rm

    # 删除所有标签为none的镜像
    docker images|grep \<none\>|awk '{print $3}'|xargs docker rmi

    # 查找容器IP地址
    docker inspect 容器名或ID | grep "IPAddress"

    # 创建网段, 名称: mynet, 分配两个容器在同一网段中 (这样子才可以互相通信)
    docker network create mynet
    docker run -d --net mynet --name container1 my_image
    docker run -it --net mynet --name container1 another_image
    ```

> 更多帮助信息 `docker-compose -h|--help`      

### 5. 目录结构

```
dockerfiles
    |-- services                    # docker 相关服务
    |-- **.yml                      # 通用配置文件
    |-- mirrors                     # source.list 镜像源地址
~/app                               # 工作源码存放目录
```


## Version 3 (docker-composer) 不再支持参数说明

**depends_on**

> 笔者解读: 通过配置 `networks` 参数更好地改进

- `depends_on` does not wait for `db` and `redis` to be “ready” before starting `web` - only until they have been started. If you need to wait for a service to be ready, see [Controlling startup order](https://docs.docker.com/compose/startup-order/) for more on this problem and strategies for solving it.
- Version 3 no longer supports the `condition` form of `depends_on`.
- The `depends_on` option is ignored when [deploying a stack in swarm mode](https://docs.docker.com/engine/reference/commandline/stack_deploy/) with a version 3 Compose file.

> @https://docs.docker.com/compose/compose-file/

**links**

**Warning**: The `--link` flag is a legacy feature of Docker. It may eventually be removed. Unless you absolutely need to continue using it, we recommend that you use [user-defined networks](https://docs.docker.com/engine/userguide/networking//#user-defined-networks) to facilitate communication between two containers instead of using `--link`. One feature that user-defined networks do not support that you can do with `--link` is sharing environmental variables between containers. However, you can use other mechanisms such as volumes to share environment variables between containers in a more controlled way.


## 各系统软件源

### Ubuntu

| 系统代号 | 版本  |
| -------- | ----- |
| precise  | 12.04 |
| trusty   | 14.04 |
| vivid    | 15.04 |
| xenial   | 16.04 |
| zesty    | 17.04 |

### Debian

| 系统代号 | 版本 |
| -------- | ---- |
| squeeze  | 6.x  |
| wheezy   | 7.x  |
| jessie   | 8.x  |
| stretch  | 9.x  |
| buster   | 10.x |

> **NOTE**:  
> 查询自己的Linux版本 `cat /etc/issue`

## git ssh 密钥配置

For your host machine which run git, all the contents of `git config --list` is stored in files:

- If use `git config --system` to configure them, they are stored in `/etc/gitconfig`
- If use `git config --global` to configure them, they are stored in `~/.gitconfig`

> @https://stackoverflow.com/questions/52819584/copying-local-git-config-into-docker-container
> @https://github.com/tomwillfixit/atomci/blob/master/docker-compose.yml

示例: 
```
volumes:
    # Git and ssh config
    - ~/.ssh:/root/.ssh:ro # Change - ssh key needed to push to github
    - ~/.gitconfig:/root/.gitconfig:ro  # Change - git config needed for user details
    #- /tmp/ssh_auth_sock:/tmp/ssh_auth_sock #Static - needed to push to github without prompt
```

## 贡献名单

... 期待你的留名 ...

##  参考资料
- [[官方文档] Dockerfile reference](https://docs.docker.com/engine/reference/builder/)
- [[官方文档] Compose file version 3 reference](https://docs.docker.com/compose/compose-file/)
- [[官方文档] Use volumes](https://docs.docker.com/storage/volumes/)
- [[官方文档] env-file](https://docs.docker.com/compose/env-file/)
- [[镜像] mysql 镜像说明](https://hub.docker.com/_/mysql/)
- [[镜像] php 镜像说明](https://hub.docker.com/_/php/)
- [[镜像站] 阿里云开源镜像站](https://opsx.alibaba.com/mirror)
- [[镜像站] 腾讯开源镜像站](https://mirrors.cloud.tencent.com/index.html)
- [[镜像站] 网易开源镜像站](http://mirrors.163.com/)
- [[镜像站] 清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/debian/)