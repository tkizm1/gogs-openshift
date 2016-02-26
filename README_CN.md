[Read English](README.md)

使用第一种方式安装单独的gogs快速简洁，在Windows平台运行rhc命令需要安装Ruby和Git并添加PATH变量。

经过个人的Win10系统安装测试：
```
Git版本可随意

Ruby2.0以及以上的版本似乎安装rhc后无法使用rhc命令。需要安装Ruby 1.9.3-p551才可以正常使用。
```
[详细安装rhc命令](http://www.freehao123.com/new-openshift/#toc-5)

只是搭建使用DIY和mysql
[![Join the chat at https://gitter.im/tkisme/gogs-openshift](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/tkisme/gogs-openshift?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
```
 rhc app create diy diy-0.1 --from-code https://github.com/Sonic853/gogs-openshift
 #可以选择安装的mysql
 rhc cartridge add mysql-5.5
 #删除这仍然是简单的
 rhc app delete diy --confirm
```

go占用286M,gogs占用38M
```
rhc app-show --gears quota
```

以上是第一种方式安装的方法

如果你想从源代码编译，可以使用第二种方式安装：以SSH方式链接项目。可以使用rhc命令连接，也可以使用Xshell5连接（Xshell5可以跳过第一条命令
```
rhc ssh
cd $OPENSHIFT_DATA_DIR/gogs
export GOROOT=$OPENSHIFT_DATA_DIR/go
export PATH=$PATH:$GOROOT/bin
export GOPATH=$OPENSHIFT_DATA_DIR/gocode
mkdir -p $GOPATH/src/github.com/gogits
cd $GOPATH/src/github.com/gogits
git clone -b develop https://github.com/gogits/gogs
cd gogs
go get -u ./...
go get -u -tags "sqlite redis memecache" github.com/gogits/gogs
go build -tags "sqlite redis memecache"
cd scripts
./build_linux64.sh
ls
mv output_amd64/ $OPENSHIFT_DATA_DIR
cd $OPENSHIFT_DATA_DIR
cp -r output_amd64/* gogs/
#删除多余文件，或者你有足够的磁盘空间
rm -rf gocode
cp -r output_amd64/* gogs/
rm -rf output_amd64
gear restart
```
