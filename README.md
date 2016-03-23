Just works use diy and mysql
[![Join the chat at https://gitter.im/tkisme/gogs-openshift](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/tkisme/gogs-openshift?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
```
 rhc app create diy diy-0.1 --from-code https://github.com/tkisme/gogs-openshift
 #optional choose mysql
 rhc cartridge add mysql-5.5
 #delete this is still simple
 rhc app delete diy --confirm
```

go use 286M,gogs use 38M
```
rhc app-show --gears quota
```

If you want build from source
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

if you want gogs support ssh on openshift,just [try edit gogs/repo.go](https://github.com/gogits/gogs/blob/master/models/repo.go)
