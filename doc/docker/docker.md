### linux 官方脚本安装
`curl -fsSL https://test.docker.com -o test-docker.sh`

`sudo sh test-docker.sh`


### windows ubuntu 安装docker
- https://blog.csdn.net/qq_37705525/article/details/127767770


### 修改pull仓库

修改或创建daemon.json文件：vi /etc/docker/daemon.json

将以下配置写入到文件中，保存并退出（不会操作的百度下vi命令吧）：

```text
{

"registry-mirrors": ["http://hub-mirror.c.163.com", "https://docker.mirrors.ustc.edu.cn", "https://registry.docker-cn.com/"]

}

```

#### 重启docker：

root\@local:/# `systemctl restart docker.service`

#### 启动docker 后台服务

root\@local:/# `service docker start`

#### 查看docker服务

root\@local:/# `ps -ef |grep docker`

#### 测试是否成功：

root\@local:/# `docker version`


#### 常用 docker run 参数
- 设置日志最大值

  `--log-opt mode=non-blocking --log-opt max-buffer-size=50m`
- 后台运行

  `-d`
- 死循环

  `docker run -d centos /bin/sh -c "while true; do echo hello world; sleep 1; done"`

#### 复制文件或文件夹
    - 将tool文件夹从容器内复制到宿主机当前目录的tool下
        docker cp sp:/tool/ ./tool/
    - other
        docker cp ./tool/ sp:/tool/
        docker cp sp:/tool/1.txt ./1.txt

#### 进入容器
    docker exec -it sp /bin/sh

#### 设置日志最大值
    docker run --log-opt mode=non-blocking --log-opt max-buffer-size=50m --name=sp -d spider:hn



### docker组件
    1. docker仓库配置文件：
        wget https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/
        vim docker-ce.repo
        %s@https://download.docker.com/@https://mirrors.tuna.tsinghua.edu.cn/docker-ce/@     

	2. 自定义docker0桥网络信息：/etc/docker/daemon.json
	{
		"bip": "192.168.1.5/24", // 指明docker0桥ip地址和24掩码,系统会自动推算出这个桥所属网络,将下列值自动修改
		"fixed-cidr": "10.20.0.0/16",
		"fixed-cidr-v6": "2001:db8:abcd::/64",
		"mtu": 1500, 
		"defailt-gateway": "10.20.1.1", //默认网关
		"defailt-gateway-v6": "2001:db8:abcd::89", //ipv6网关
		"dns": ["10.20.1.2", "10.20.1.3"] //dns服务器地址
	}

	3. 配置docker镜像加速代理：/etc/docker/daemon.json
	{
		"registry-mirrors":["https://registry.docker-cn.com"]
	}

	4. 允许别人从外部链接(tcp协议)进docker容器：/etc/docker/daemon.json
	{
		"host":["tcp://0.0.0.0:2375", "unix:///var/run/docker.sock"]
	}
	docker.sock默认链接本机