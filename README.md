# 从 Docker 安装 flowci

## 安装 Docker 环境

> 具体的安装步骤请查看 [Docker 官方文档](https://docs.docker-cn.com/engine/installation/)

## 从 Docker Hub 镜像启动

flowci 在 Docker Hub 上提供了最新的镜像，用户可以方便的获取最新的镜像并开始 flowci 之旅。

> 国内直接拉取 Docker Hub 会很慢, 加速请参考 [配置Docker Hub 加速器](https://www.docker-cn.com/registry-mirror), 或者自行配置阿里云等镜像

### 第一步 克隆本仓库

仓库中提供了快速启动以及相关的服务器配置，可以通过以下两种方式克隆本仓库:
	
- 通过 Git 的形式 Clone 代码，确保机器已经安装了 Git
   
   `git clone https://github.com/FlowCI/docker.git`
	  
- 或者直接通过 http 下载的形式下载代码，之后解压缩
	   
   `curl -L -o docker.zip https://github.com/FlowCI/docker/archive/master.zip`
   
> 如果此仓库已经克隆过，在更新版本时需要使用 `git pull https://github.com/FlowCI/docker.git` 获取最新的内容

### 第二步 从 Docker 启动 flowci

进入到上一步获取的代码目录，并执行 `./start-services.sh`， 之后可以访问 `http://localhost:3000` 进入 flowci。
 
> 环境变量的设置:
> 
> - `FLOW_API_DOMAIN`： 部署的后端 API 域名地址， 为 8080 端口， 默认：`127.0.0.1`
> - `FLOW_WEB_DOMAIN`： 部署的前端 Web 页面的域名地址，为 3000 端口，默认：`127.0.0.1`
> - `FLOW_SYS_EMAIL`：flowci 系统管理员账号，默认是 `admin@flow.ci `(第一次初始化之后不可修改)
> - `FLOW_SYS_USERNAME`：flowci 系统管理员的用户名，默认是 `admin` (第一次初始化之后不可修改)
> - `FLOW_SYS_PASSWORD`: flowci 系统管理员密码，默认是 `123456`
> - `MYSQL_PASSWORD`： flowci MYSQL 数据库 `root` 用户的密码，默认为 `flowci`, 
> - `MYSQL_HOST`：flowci MYSQL 数据库的HOST，默认是 `127.0.0.1`
> - `MYSQL 的存储路径`: `~/flow-ci/db` 如果正式部署请在 docker-compose.yml 修改 MYSQL 的数据存储位置
> - `flow.ci 的数据存储路径`: `~/flow-ci/data` 如果正式部署请在 docker-compose.yml 修改 flow.ci 的数据存储位置

例如：配置的域名为 `yourhost.com`，则可以通过以下命令启动:

```bash
FLOW_API_DOMAIN=yourhost.com FLOW_WEB_DOMAIN=yourhost.com ./start-services.sh
```

## 启动 Agent 

> 需要替换的环境变量:
> 
> - `FLOW_API_DOMAIN`： 为所配置的 API 的域，例如 `127.0.0.1`
> - `FLOW_TOKEN`:  Agent 启动令牌，如何获取请参见 [ Agent 管理 ](https://github.com/FlowCI/docs/blob/master/admin_agent.md)


- 以 Docker 方式启动
 
  `USE_DOCKER=true ./start-agent.sh $FLOW_API_DOMAIN $FLOW_TOKEN`

- Java 方式启动
  > 需要准备 Java 1.8 的环境
  
  `./start-agent.sh $FLOW_API_DOMAIN $FLOW_TOKEN`

### 常见问题
>- 端口修改，[请查看文档](https://github.com/FlowCI/docs/blob/master/cf_docker.md)
>- Mysql启动失败，请检查 docker-compose 挂载的数据库存储目录的权限既`~/flow-ci/db`的权限或者直接在 docker-compose.yml 加上 privileged: true(是使你的docker用户拥有真正的root的权限，建议慎用)
>- Mysql启动成功但是Tomcat启动失败，请查看 Jvm 堆大小的分配，默认是1.5G可酌情配置 -Xms512M -Xmx512M  

