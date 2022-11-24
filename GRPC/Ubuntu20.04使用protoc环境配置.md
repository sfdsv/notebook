protoc环境配置包含protoc、Grpc-gateway等方面的配置。

## Protoc

github地址：

https://github.com/protocolbuffers/protobuf

安装步骤：
1. 下载合适的压缩包文件
2. 解压
3. 根据readme.txt，将bin文件里的可执行文件protoc移动到/usr/local/bin里


## Grpc-gateway

github地址：

https://github.com/grpc-ecosystem/grpc-gateway

安装步骤：
```bash
go install github.com/grpc-ecosystem/grpc-gateway/v2/protoc-gen-grpc-gateway@latest
go install github.com/grpc-ecosystem/grpc-gateway/v2/protoc-gen-openapiv2@latest 
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest 
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
```

> 可生成文档的插件：https://github.com/pseudomuto/protoc-gen-doc
