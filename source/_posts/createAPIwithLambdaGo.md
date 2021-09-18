---
title: Go and gRPC
date: 2021-07-02 22:25:00
tags:
- GO
- gPPC
clearReading: true
autoThumbnailImage: yes
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Back-end Knowledge
---
Go 和 gRPC的入门尝试

<!--more-->
### Go是什么
Go语言是Google社内开发的一个开源编程语言。
Go的优点是对初学者友好,处理速度快,所需代码量少,有丰富的库以及插件,还可以进行并发处理,安全性高。
Go的缺点是情报较少,不支持Generics,没有例外处理,代码无法进行继承。

### gRPC是什么
gRPC是由Google公司开源的一款高性能的远程过程调用(RPC)框架，可以在任何环境下运行。该框架提供了负载均衡，跟踪，智能监控，身份验证等功能，可以实现系统间的高效连接。

##### ※RPC
Remote Procedure Call的简称，翻译成中文就是远程过程调用。
也就是说两台服务器A,B,一个应用部署在A服务器上,想要调用B服务器上应用提供的函数/方法,由于不在一个内存空间,不能直接调用,需要通过网络来表达调用的语义和传达调用的数据。

### 什么是Protocol Buffers
Protocol Buffers是Google开源的一个语言无关、平台无关的通信协议，其小巧、高效和友好的兼容性设计，使其被广泛使用。

### 使用Go来玩玩gRPC
grpc.io提供的Go来玩一下gRPC.根据指导来.
首先确认一下Go的版本,需要Go1.6以上的。
```
$ go version
go version go1.12.6 darwin/amd64
```
安装gRPC
```
$ go get -u google.golang.org/grpc

$ cat $GOPATH/src/google.golang.org/grpc/version.go | grep Version
// Version is the current grpc version.
const Version = "1.23.0-dev"
```

下载Protocol Buffers v3
```
$ brew install protobuf

$ protoc --version
libprotoc 3.7.1
```

下载Protocol Buffers的Go插件
```
go get -u github.com/golang/protobuf/protoc-gen-go
```

Protocol Buffers文件(.proto)里定义service和message
在说明service和service里的rpc方法(也叫做service message)之前先说明一下gRPC能够定义的4种方法。

- 1. Simple RPC
gRPC客户端发送的单一消息到gRPC服务器,服务器再返回单一消息。(和tutorial的`GetFeature`相当)

- 2. Server side Streaming RPC
当gRPC客户端发来单一消息的时候gRPC服务器从流式返回多条消息。(和tutorial的`ListFeatures`相当)

- 3. Client side Streaming RPC
当gRPC客户端发来多条消息的时候gRPC服务器返回单一消息。(和tutorial的`RecordRoute`相当)

- 4. 双向 Streaming RPC
当gRPC客户端发来多条消息的时候gRPC服务器从流式返回多条消息。(和tutorial的`RouteChat`相当)

※ Steaming(串流)
>串流技术，就是通过网路实时压缩和传输影音的技术。好处就是你不需要把完整的多媒体资料下载完后才能观看，而是像"水流"一样源源不断实时从发布源传到客户端。经过串流技术处理的、可以实时播放的多媒体有一个耳熟能详的名字，叫流媒体。

那我们就根据上面的种类来定义service和service method。
tutorial定义的是`route_guide.proto`

可以理解为「gRPC客户端发送的Request = service methord的参数」而「gPRC服务器返回的Response = service methord的返回值」，当使用service methord的Streaming的时候需要在参数和返回值之前加上`stream`声明。

```
service RouteGuide {
  rpc GetFeature(Point) returns (Feature) {}
  rpc ListFeatures(Rectangle) returns (stream Feature) {}
  rpc RecordRoute(stream Point) returns (RouteSummary) {}
  rpc RouteChat(stream RouteNote) returns (stream RouteNote) {}
}
```

接下来可以定义message,使用service methord来定义所使用的message的类型。

```
message Point {
  int32 latitude = 1;
  int32 longitude = 2;
}

message Rectangle {
  Point lo = 1;
  Point hi = 2;
}

message Feature {
  string name = 1;
  Point location = 2;
}

message RouteNote {
  Point location = 1;
  string message = 2;
}

message RouteSummary {
  int32 point_count = 1;
  int32 feature_count = 2;
  int32 distance = 3;
  int32 elapsed_time = 4;
}
```

从Protocol Buffers文件（.proto） 能够自动生成Go文件（.pb.go).

Protocol Buffers文件编译之后，将会自动生成Go文件，该文件包括gRPC客户端代码和已经编译好的gRPC服务端的接口。

执行`protoc`命令行的话将会根据`route_guide.proto`里的定义自动生成`route_guide.pb.go`。

根据`--proto_path`来指定编译对象的Protocol Buffers文件路径,`--go_out`来指定自动生成的Go文件的输出路径(为了将gRPC设置成插件，要记得定义好plugins=grpc).

```
$ protoc --proto_path routeguide/ routeguide/route_guide.proto --go_out=plugins=grpc:routeguide

```

自动生成的Go文件(route_guide.pb.go)包含下面的东西。

#### ①.针对定义好的message的生成的构造体
在构造体内也包含着对数据进行操作的方法。

```
type Point struct {
	Latitude             int32    `protobuf:"varint,1,opt,name=latitude,proto3" json:"latitude,omitempty"`
	Longitude            int32    `protobuf:"varint,2,opt,name=longitude,proto3" json:"longitude,omitempty"`
	XXX_NoUnkeyedLiteral struct{} `json:"-"`
	XXX_unrecognized     []byte   `json:"-"`
	XXX_sizecache        int32    `json:"-"`
}

func (m *Point) GetLatitude() int32 {
	if m != nil {
		return m.Latitude
	}
	return 0
}

func (m *Point) GetLongitude() int32 {
	if m != nil {
		return m.Longitude
	}
	return 0
}
```

#### ②.呼出service methord的gRPC客户端代码
以下是用于返回gRPC客户端的接口和各种处理的客户端的代码。
```
type RouteGuideClient interface {
	GetFeature(ctx context.Context, in *Point, opts ...grpc.CallOption) (*Feature, error)
	ListFeatures(ctx context.Context, in *Rectangle, opts ...grpc.CallOption) (RouteGuide_ListFeaturesClient, error)
	RecordRoute(ctx context.Context, opts ...grpc.CallOption) (RouteGuide_RecordRouteClient, error)
	RouteChat(ctx context.Context, opts ...grpc.CallOption) (RouteGuide_RouteChatClient, error)
}

type routeGuideClient struct {
	cc *grpc.ClientConn
}

func NewRouteGuideClient(cc *grpc.ClientConn) RouteGuideClient {
	return &routeGuideClient{cc}
}
```

下面是利用上面的接口来便携的用于呼出service methord `GetFeature`的方法.`c.cc.Invoke`的第二参数是gPRC的service methord的endpoint.

```
func (c *routeGuideClient) GetFeature(ctx context.Context, in *Point, opts ...grpc.CallOption) (*Feature, error) {
	out := new(Feature)
	err := c.cc.Invoke(ctx, "/routeguide.RouteGuide/GetFeature", in, out, opts...)
	if err != nil {
		return nil, err
	}
	return out, nil
}
```

上面的例子是呼出simple RPC service methord的`GetFeatuer`的简单的例子.如果涉及到串流的话可能写法会有一些不同.比如我们来看看双向串流的RPC的`RouteChat`的客户端代码.

因为是呼出双向串流的RPC的`RouteChat`的代码，所以service methord的呼出结果(来自gRPC服务器的response)是串流所接收的结果。

下面我们可以来看看下面的客户端的代码,我们可以推断出：service methord的呼出结果的返回值`RouteGuide_RouteChatClient`是串流发送message的`Send`(gPRC客户端 => gRPC服务器)、串流用于接收message的`Recv`(gRPC服务器 => gRPC客户端)。

```
func (c *routeGuideClient) RouteChat(ctx context.Context, opts ...grpc.CallOption) (RouteGuide_RouteChatClient, error) {
	stream, err := c.cc.NewStream(ctx, &_RouteGuide_serviceDesc.Streams[2], "/routeguide.RouteGuide/RouteChat", opts...)
	if err != nil {
		return nil, err
	}
	x := &routeGuideRouteChatClient{stream}
	return x, nil
}

type RouteGuide_RouteChatClient interface {
	Send(*RouteNote) error
	Recv() (*RouteNote, error)
	grpc.ClientStream
}

type routeGuideRouteChatClient struct {
	grpc.ClientStream
}

func (x *routeGuideRouteChatClient) Send(m *RouteNote) error {
	return x.ClientStream.SendMsg(m)
}

func (x *routeGuideRouteChatClient) Recv() (*RouteNote, error) {
	m := new(RouteNote)
	if err := x.ClientStream.RecvMsg(m); err != nil {
		return nil, err
	}
	return m, nil
}
```

这里定义的是从gRPC服务器的response到gRPC客户端的串流接收为止.

#### ③.service methord用的gRPC服务器用的接口
和客户端的代码不同,gRPC服务器是需要自己事先进行编写，所以只是包含了接口的定义.

```
type RouteGuideServer interface {
	GetFeature(context.Context, *Point) (*Feature, error)
	ListFeatures(*Rectangle, RouteGuide_ListFeaturesServer) error
	RecordRoute(RouteGuide_RecordRouteServer) error
	RouteChat(RouteGuide_RouteChatServer) error
}
```

simple RPC的`GetFeature`的参数的类型是，Protocol Buffers文件的定义里包含的东西，所以和串流相关的service methord的参数是略微不同,我们来挖一挖双向串流RPC的`RouteChat`。

`RouteChat`的参数`RouteGuide_RouteChatServer`如下，串流用于送行的`Send`(gRPC服务器 => gRPC客户端)和`Recv`(gRPC客户端 => gRPC服务器)

```
type RouteGuide_RouteChatServer interface {
	Send(*RouteNote) error
	Recv() (*RouteNote, error)
	grpc.ServerStream
}

type routeGuideRouteChatServer struct {
	grpc.ServerStream
}

func (x *routeGuideRouteChatServer) Send(m *RouteNote) error {
	return x.ServerStream.SendMsg(m)
}

func (x *routeGuideRouteChatServer) Recv() (*RouteNote, error) {
	m := new(RouteNote)
	if err := x.ServerStream.RecvMsg(m); err != nil {
		return nil, err
	}
	return m, nil
}
```

### gRPC服务器的编写
要编写gRPC服务器的话需要以下的工作:
1. 在自动生成的Go文件里添加gRPC服务器的接口.
2. 从gRPC客户端的request到gRPC服务器送去request的处理.

gRPC服务器的接口的编写
```
type routeGuideServer struct {
    ...
}
func (s *routeGuideServer) GetFeature(ctx context.Context, point *pb.Point) (*pb.Feature, error) {
    ...
}
func (s *routeGuideServer) ListFeatures(rect *pb.Rectangle, stream pb.RouteGuide_ListFeaturesServer) error {
    ...
}
func (s *routeGuideServer) RecordRoute(stream pb.RouteGuide_RecordRouteServer) error {
    ...
}
func (s *routeGuideServer) RouteChat(stream pb.RouteGuide_RouteChatServer) error {
    ...
}
```


