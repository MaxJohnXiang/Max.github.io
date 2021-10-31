---
title: "用使Go Micro写微服务"
date: 2019-12-25T18:13:06+08:00
draft: false
---


##介绍
go-micro 是微服务开发组件库， 提供一些方便的功能.
1. 服务发现
2. 负载均衡
3. 消息编码
4. 异步消息
5. 可插拔接口


## 安装

1. go 依赖

```go
brew install protobuf
```

安装php 依赖
```
PROTOC_ZIP=protoc-3.7.1-linux-x86_64.zip
curl -OL https://github.com/protocolbuffers/protobuf/releases/download/v3.7.1/$PROTOC_ZIP
sudo unzip -o $PROTOC_ZIP -d /usr/local bin/protoc
sudo unzip -o $PROTOC_ZIP -d /usr/local 'include/*'
rm -f $PROTOC_ZIP
```


```
go get -u -v github.com/golang/protobuf/{proto,protoc-gen-go}
go get -u -v github.com/micro/protoc-gen-micro
```

php 相关依赖
```
# 安装 php 扩展
pecl install protobuf
pecl install grpc
git clone -b $(curl -L https://grpc.io/release) https://github.com/grpc/grpc
cd grpc
git submodule update --init
make
[sudo] make install
# 在./bin 目录下会生成 编译 php grpc协议时需要的 中间件 (二进制文件)
```

## 写proto文件


1. 首先第一行语法版本 

```
syntax = "proto3";
```

2. 使用message 定义结构  
```
message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
} 
```

3. 可以使用自定义结构

```

message SearchResponse {
  repeated Result results = 1;
}
message Result {
  string url = 1;
  string title = 2;
  repeated string snippets = 3;
}
```

3. 可以导入proto


4. 定义接口
```go
service SearchService {
  rpc Search (SearchRequest) returns (SearchResponse);
  rpc Update (SearchRequest) returns (SearchResponse);
  rpc Delete (SearchRequest) returns (SearchResponse);
  rpc Insert (SearchRequest) returns (SearchResponse);
}
```


5. 生成代码文件


```
protoc --proto_path=. --go_out=. --micro_out=. prime/prime.proto
```

```
└── sum
    ├── sum.pb.go
    ├── sum.pb.micro.go
    └── sum.proto
```

6. 然后开始实现proto文件的接口
  1. 定义一个handler struct 
    ```proto
      type handler struct {
      }
    ```
  2. 实现接口方法
    ```go
	func (h handler) GetSum(_ context.Context, req *sum.SumRequest, res *sum.SumResponse) error {
		inputs := make([]int64, 0)

		var i int64 = 0
		fmt.Println("req.Input", req.Input)
		for ; i <= req.Input; i++ {
			inputs = append(inputs, i)
		}
		fmt.Println("inputs", inputs)

		res.Output = service.GetSum(inputs...)
		return nil
	}
    ```

  3. 服务注册
    ```go
	/new 一个server
    	srv := micro.NewService(
		micro.Name("go.micro.srv.sum"),
	)

	//服务初始化
	srv.Init()
	//挂载接口
	注册服务， 并且传入 实现的类
	_ = sum.RegisterSumHandler(srv.Server(), handler.Handler())

	if err := srv.Run(); err != nil {
		panic(err)
	}
      ```
  

## 客户端调用

```go

	srv := web.NewService(
		web.Name("go.micro.web.portal"),
		web.Address(":8888"),
		web.StaticDir("html"),
	)
	srv.Init()
	//初始话服务客户端
	srvClient = sum.NewSumService("go.micro.srv.sum", srv.Options().Service.Client())
	//传入参数
	rsp, err := srvClient.GetSum(context.Background(), req)
```
