# gogs-openshift

## Instructions
Just works use diy
[![Join the chat at https://gitter.im/tkisme/gogs-openshift](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/tkisme/gogs-openshift?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
```
 # execute it on local,not in server
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

## Upgrade gogs
```
 git remote add github git@github.com:tkisme/gogs-openshift.git
 git pull github master
 git push
```

## Upgrade by your own
this is just for people who want to upgrade in hurry
```
 vi .openshift/action_hooks/start
 git commit -am "upgrade gogs"
 git push
```
change download_url to anyversion you want

I change [v0.8.43 to v0.9.13](https://github.com/tkisme/gogs-openshift/commit/5401b1ec672ac38bd08cf50afb000f7e5fce6a0c)

Now it upgrade gogs to v0.9.13


## Build from source
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

## Fix git clone ssh link of gogs 
if you want gogs support ssh on openshift,just [try edit gogs/repo.go](https://github.com/gogits/gogs/blob/master/models/repo.go#L1084)

```
func RepoPath(userName, repoName string) string {
// EDIT HERE!!!!!
	return filepath.Join(UserPath(userName), strings.ToLower(repoName)+".git")
}
```
