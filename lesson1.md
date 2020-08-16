### 编译安装

#### pd

源码目录下直接make

#### tikv

安装好rust，源码目录下直接make，发现卡在fetch rust-protobuf。在```~/.cargo/config```文件里加上如下配置：
```
[net]
retry = 2
git-fetch-with-cli = true
```
再make一次，成功

#### tidb

源码目录下直接make




### 启动服务

#### 启动pd-server

pd目录下执行```bin/pd-server```，有如下输出，可知服务启动成功


#### 启动tikv-server

tikv目录下执行
```shell
./target/release/tikv-server
```

最后一行输出报
```
[2020/08/16 20:18:13.154 +08:00] [FATAL] [server.rs:920] ["the maximum number of open file descriptors is too small, got 256, expect greater or equal to 82920"]
```

得知是max open file设得太低，执行以下命令放宽限制

```shell
sudo sysctl -w kern.maxfiles=1048600
sudo sysctl -w kern.maxfilesperproc=1048576
sudo ulimit -n 82920
```
再次启动服务，看到如下输出
```
[2020/08/16 20:30:20.145 +08:00] [INFO] [server.rs:244] ["TiKV is ready to serve”]
```
可知服务启动成功


#### 启动tidb-server

tidb目录下执行```bin/tidb-server```，看到如下输出，可知服务启动成功

