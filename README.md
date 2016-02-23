只是搭建使用DIY和mysql
[![Join the chat at https://gitter.im/tkisme/gogs-openshift](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/tkisme/gogs-openshift?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
```
 rhc app create diy diy-0.1 --from-code https://github.com/tkisme/gogs-openshift
 #可以选择安装的mysql
 rhc cartridge add mysql-5.5
 #删除这仍然是简单的
 rhc app delete diy --confirm
```

go占用286M,gogs占用38M
```
rhc app-show --gears quota
```

如果你想从源代码编译
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
#let source go,or you have enough disk space
rm -rf gocode
cp -r output_amd64/* gogs/
rm -rf output_amd64
gear restart
```
