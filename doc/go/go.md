## go环境配置

*   GOROOT ： golang安装目录 D:\programme\GO
*   GOPATH  ：保存源码路径  D:\_document\Go\_programe\gopath
*   path ： D:\programme\GO\bin

## 代理配置

*   GOPROXY=<https://goproxy.io/>
*   GOPROXY=<https://goproxy.cn>
*   GOPROXY=<https://mirrors.aliyun.com/goproxy/>

## 清除所有编译文件和关联包

*   go clean -modcache

## 编译

*   set GOARCH=amd64
*   set GOOS=linux
*   go build



## Linux

*   wget <https://dl.google.com/go/go1.16.5.linux-amd64.tar.gz>
*   sudo tar -C /usr/local/ -xzvf go1.16.5.linux-amd64.tar.gz
*   vim /etc/profile
*   export GOROOT=/usr/local/go
*   export GOPATH=/download/gopath
*   export GOBIN=\$GOPATH/bin
*   export PATH=$PATH:$GOROOT/bin
*   export PATH=$PATH:$GOPATH/bin
*   export GO111MODULE=on
*   export GOPROXY=<https://goproxy.io>

## Mac

*   /bin/zsh -c "\$(curl -fsSL <https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh>)"
*   brew install go
*   vim /Users/cc/.profile
*   export GOPATH=/download/gopath
*   export GOBIN=\$GOPATH/bin
*   export PATH=$PATH:$GOROOT/bin
*   export PATH=$PATH:$GOPATH/bin
*   export GO111MODULE=on
*   export GOPROXY=<https://goproxy.io>

## 远程

1.  安装dlv

- git clone https://github.com/go-delve/delve.git $GOPATH/src/github.com/go-delve/delve
- go install github.com/go-delve/delve/cmd/dlv
- ln -s $GOPATH/bin/dlv /usr/local/bin/dlv

1.  结束调试

*   kill -9 `ps -ef | grep "dlv|mytest" -E | awk '{print $2}'`

## 单元测试

*   go test -cover
