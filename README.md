Just works use diy and mysql
```
 rhc app create diy diy-0.1
 rhc cartridge add mysql-5.5
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
