# 计算机网络

## osi七层模型和tcp/ip四层模型

![image-20250929004557942](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250929004557942.png)

![image-20250929004616460](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250929004616460.png)

# Linux常用指令与 Docker 常用命令手册

### 一、Linux 服务器基础运维命令

### 1.1 系统信息与监控

- `uname -a`：显示系统内核版本及硬件信息
- `uptime`：查看系统运行时间及负载状态
- `free -h`：查看内存使用情况（人类可读格式）
- `df -h`：显示磁盘空间使用情况
- `top/htop`：实时监控进程及资源占用（htop 需安装）
- `iostat -x 1`：查看磁盘 I/O 性能指标（每秒刷新）
- `vmstat 1`：显示内存、进程、CPU 等系统状态

### 1.2 文件与目录操作

- `ls -lah`：显示目录下文件详情（含隐藏文件）
- `cp -r source destination`：递归复制目录
- `rm -rf dirname`：强制删除目录及内容（高危指令）
- `chmod 755 filename`：修改文件权限（755 表示 rwxr-xr-x）
- `chown user:group file`：修改文件属主和属组
- `du -sh dirname`：统计目录大小
- `find / -name "filename"`：全局搜索文件
- `tree -L 2`：以树形结构显示目录层级（显示 2 层）

### 1.3 用户与权限管理

- `useradd username`：创建新用户
- `passwd username`：修改用户密码
- `usermod -aG groupname username`：将用户加入用户组
- `id username`：查看用户 UID/GID
- `usermod -L username`：锁定用户账户
- `last`：显示用户登录历史记录

### 1.4 进程管理

- `ps aux`：查看所有运行中的进程
- `kill -9 PID`：强制终止指定进程
- `pkill processname`：按进程名终止任务
- `nice -n 10 command`：调整进程优先级
- `nohup command &`：后台运行命令，终端关闭后仍继续执行

### 1.5 网络管理

- `ip a`：查看网络接口及 IP 地址（推荐替代 ifconfig）
- `netstat -tulnp/ss -tulnp`：查看端口监听状态
- `ping -c 4 host`：测试网络连通性（发送 4 个包）
- `traceroute host`：追踪网络路由路径
- `mtr hostname`：结合 ping 和 traceroute 功能，提供更详细网络诊断

### 1.6 服务管理（Systemd）

- `systemctl start service`：启动服务
- `systemctl stop service`：停止服务
- `systemctl status service`：查看服务运行状态
- `systemctl enable service`：设置服务开机自启
- `systemctl is-enabled service`：检查服务是否设置为开机启动

### 1.7 软件包管理

#### Debian/Ubuntu 系列

- `apt update`：更新软件包列表
- `apt install package`：安装软件包
- `apt remove package`：删除软件包

#### CentOS/RHEL 系列

- `yum update`：更新软件包
- `yum install package`：安装软件包
- `yum remove package`：删除软件包

------

### 二、Docker 基础语法与操作

### 2.1 Docker 安装与配置

#### Linux 一键安装脚本

bash

```bash
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
sudo systemctl enable docker
sudo systemctl start docker
```

#### 配置国内镜像加速

bash

```bash
echo '{
  "registry-mirrors": ["http://hub-mirror.c.163.com"]
}' > /etc/docker/daemon.json
systemctl daemon-reload
systemctl restart docker
```

### 2.2 镜像管理命令

- **`docker pull IMAGE_NAME`**：从仓库下载镜像
- **`docker images`**：列出本地镜像
- **`docker search mysql`**：搜索镜像
- **`docker rmi -f 镜像id`**：强制删除镜像
- **`docker tag 源镜像名:TAG 新镜像名:新TAG`**：更改镜像名和标签
- **`docker build -t your-image-name .`**：根据 Dockerfile 构建镜像

### 2.3 容器生命周期管理

- **`docker run -d -p 主机端口:容器端口 镜像名`**：运行容器（-d 后台运行，-p 端口映射）
- **`docker ps`**：查看正在运行的容器
- **`docker ps -a`**：查看所有容器（含停止的）
- **`docker stop/restart/start 容器ID`**：停止 / 重启 / 启动容器
- **`docker rm -f 容器ID`**：强制删除容器
- **`docker exec -it 容器ID /bin/bash`**：进入正在运行的容器

### 2.4 容器数据管理

- **`docker cp 容器ID:容器路径 主机路径`**：从容器拷贝文件到主机
- **`docker cp 主机路径 容器ID:容器路径`**：从主机拷贝文件到容器
- **数据卷挂载**：使用`-v`参数创建和管理数据卷

### 2.5 Dockerfile 核心指令

dockerfile











```dockerfile
FROM ubuntu:20.04              # 指定基础镜像
MAINTAINER name email          # 维护者信息
RUN apt-get update && apt-get install -y python3  # 执行命令
WORKDIR /app                   # 设置工作目录
COPY . /app                    # 复制文件到镜像
EXPOSE 8080                    # 暴露端口
CMD ["python3", "app.py"]      # 容器启动命令
```

### 2.6 实践示例

#### 部署 Nginx 服务

bash

```bash
docker search nginx
docker pull nginx
docker run -d --name nginx01 -p 8081:80 nginx
curl localhost:8081  # 测试访问
```

#### SpringBoot 应用 Docker 化

dockerfile

```dockerfile
FROM java:8
COPY *.jar /app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","/app.jar"]
```

构建命令：`docker build -t myapp .`

------

## 三、实用技巧与注意事项

### 3.1 Linux 运维技巧

- 使用`cd -`快速切换到上一个工作目录
- `ls -lh`以人类可读格式显示文件大小，`ls -lt`按修改时间排序
- 使用`tail -f /var/log/syslog`实时查看系统日志
- 设置命令别名：`alias ll='ls -l'`提高效率

### 3.2 Docker 运维技巧

- 清理无容器使用的镜像：`docker system prune -a`
- 查看容器日志：`docker logs -f --tail=行数 容器ID`
- 删除异常停止的容器：`docker rm $(docker ps -a | grep Exited | awk '{print $1}')`

> 注意：`rm -rf`和`docker rm -f`等命令具有破坏性，执行前请确认对象是否正确。建议关键操作前进行备份，生产环境谨慎使用强制删除选项。
