GoEdge边缘节点源码

## 源码编译

## 整体源码结构
从Github上对应仓库下载各个组件的源码后，建议的整体源码结构为：

EdgeProject/
   EdgeAdmin     # 管理平台
   EdgeAPI       # API节点
   EdgeNode      # 边缘节点
   EdgeCommon    # 公共依赖
   ....


源码下载地址：

* GitHub: https://github.com/orglen ，一直保持最新

## 运行环境

* 操作系统：目前只支持macOS和Linux开发环境；
* MySQL：支持 v5.7.x 以上 / TiDB 3.0以上；
* Golang：支持 v1.21及以上；
* macOS X上需要安装musl交叉编译工具链，安装方法当前文档的下一段会给出。
* 安装前需要准备wget、patch、bzip2、gcc-c++ gcc等。



### 编译EdgeNode边缘节点

从 https://github.com/orglen/EdgeNode 下载EdgeNode源码；

从 https://github.com/orglen/EdgeCommon 下载EdgeCommon源码，如果已经下载则不需要重复下载；

将EdgeNode和EdgeCommon放在同一目录下；

转到 EdgeNode 目录下；

执行 `go mod download` 下载项目依赖的源码；
复制 `build/configs/api_node.template.yaml 到 build/configs/api_node.yaml`，如果 `api_node.yaml` 已经存在则无需重复复制；然后修改其中的配置；如果你还没有边缘节点，需要先运行EdgeAdmin并通过界面创建一个节点后再修改配置后运行边缘节点；
运行 `go run -tags community cmd/edge-node/main.go`

* 商业版源码请将 -tags community 换成 -tags plus

如果想编译整个项目，请参考 `build/build.sh`，比如可以运行：

`./build.sh linux amd64`

来编译linux/amd64版本的项目压缩包，编译后生成的压缩包可以在dist目录下找到。


## 常见问题


### 下载依赖的Go模块失败

go mod download 操作可能因为网络原因失败，如果失败，建议使用网络代理尝试重新下载：

`env https_proxy=127.0.0.1:7890 go mod download`

其中的 127.0.0.1:7890 换成你自己的网络代理地址。
或
`go env-w GOPROXY=https://goproxy.cn,direct`

如何编译Freebsd上的边缘节点

先确保bash命令存在：

`bash`

如果提示 bash: Command not found 则可以使用pkg命令安装：

`pkg install bash`

检查zip是否存在，如果不存在同样：

`pkg install zip`

然后可以在Freebsd系统中直接编译（需要先下载源码EdgeCommon和EdgeNode）：

`./build.sh freebsd amd64`


### Linux 下 EdgeNode 出现以下报错 

`webp.go:47:20: too many errors  webDecodeRGBA`

是缺少 GCC 安装 GCC

`sudo apt install build-essential`

windows 参考
https://jmeubank.github.io/tdm-gcc/




## 参考资料

* [Linux上使用musl-cross-make](https://github.com/richfelker/musl-cross-make)

* 安装前需要准备wget、patch、bzip2、gcc-c++等。
