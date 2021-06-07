# gRPC

### 流式

* Server-side streaming RPC: 服务器端流式 RPC
* Client-side streaming RPC: 客户端流式 RPC
* Bidirectional streaming RPC: 双向流式 RPC

### 为什么不用 Simple RPC

流式为什么要存在呢，是 Simple RPC 有什么问题吗？通过模拟业务场景，可得知在使用 Simple RPC 时，有如下问题：

- 数据包过大造成的瞬时压力
- 接收数据包时，需要所有数据包都接受成功且正确后，才能够回调响应，进行业务处理（无法客户端边发送，服务端边处理）

### 为什么用 Streaming RPC

- 大规模数据包
- 实时场景

## Protobuf

### v2 和 v3 主要区别

- 删除原始值字段的字段存在逻辑
- 删除 required 字段
- 删除 optional 字段，默认就是
- 删除 default 字段
- 删除扩展特性，新增 Any 类型来替代它
- 删除 unknown 字段的支持
- 新增 [JSON Mapping](https://developers.google.com/protocol-buffers/docs/proto3#json)
- 新增 Map 类型的支持
- 修复 enum 的 unknown 类型
- repeated 默认使用 packed 编码
- 引入了新的语言实现（C＃，JavaScript，Ruby，Objective-C）

### 相较 Protobuf，为什么不使用 XML/JSON？

- 更简单
- 数据描述文件只需原来的 1/10 至 1/3
- 解析速度是原来的 20 倍至 100 倍
- 减少了二义性
- 生成了更易使用的数据访问类

### Package

在 proto 文件中使用 `package` 关键字声明包名, 默认转换成 go 中的包名与此一致, 如果需要指定不一样的包名, 可以使用 `go_package` 的选项

```protobuf
package test;
option go_package="mytest";
```

使用**包名+消息名**的方式来使用类型

```protobuf
// foo.proto
syntax = "proto3";
package foo.bar;
message Open {...}

// bar.proto
message Foo {
	foo.bar.Open open = 1;
}
```

### Message

proto 中的 `message` 对应 go 中的 `struct`, 全部使用驼峰命名规则. 嵌套定义的 `message`, `enum` 转换为 go 之后, 名称变为 Parent_Child 结构.

```protobuf
// proto
message Test {
    int32 age = 1;
    int64 count = 2;
    double money = 3;
    float score = 4;
    string name = 5;
    bool fat = 6;
    bytes char = 7;
    // Status 枚举状态
    enum Status {
        OK = 0;
        FAIL = 1;
    }
    Status status = 8;
    // Child 子结构
    message Child {
        string sex = 1;
    }
    Child child = 9;
    map<string, string> dict = 10;
}
```

```go
// go
type Test_Status int32
const (
    Test_OK   Test_Status = 0
    Test_FAIL Test_Status = 1
)
// Test 测试
type Test struct {
    Age    int32       `protobuf:"varint,1,opt,name=age" json:"age,omitempty"`
    Count  int64       `protobuf:"varint,2,opt,name=count" json:"count,omitempty"`
    Money  float64     `protobuf:"fixed64,3,opt,name=money" json:"money,omitempty"`
    Score  float32     `protobuf:"fixed32,4,opt,name=score" json:"score,omitempty"`
    Name   string      `protobuf:"bytes,5,opt,name=name" json:"name,omitempty"`
    Fat    bool        `protobuf:"varint,6,opt,name=fat" json:"fat,omitempty"`
    Char   []byte      `protobuf:"bytes,7,opt,name=char,proto3" json:"char,omitempty"`
    Status Test_Status `protobuf:"varint,8,opt,name=status,enum=test.Test_Status" json:"status,omitempty"`
    Child  *Test_Child `protobuf:"bytes,9,opt,name=child" json:"child,omitempty"`
    Dict   map[string]string `protobuf:"bytes,10,rep,name=dict" json:"dict,omitempty" protobuf_key:"bytes,1,opt,name=key" protobuf_val:"bytes,2,opt,name=value"`
}
// Child 子结构
type Test_Child struct {
    Sex string `protobuf:"bytes,1,opt,name=sex" json:"sex,omitempty"`
}
```

#### 标签

[1-15] 内的标识在编码时占用一个字节, 包含标识和字段类型. [16-2047] 之间的标识符占用两个字节. 建议为频繁出现的消息元素分配 [1-15] 间的标签.

标签范围: [1, 2^29^ - 1], 不可使用 [19000, 19999] 之间的标识符, protobuf 协议预留了这些标识符.

#### 关键字

- `repeated`: 前置 `repeated` 关键字, 声明改字段为数组类型.

- `reserved`: 指定保留字段名和标签, 不可以在一个 `reserved` 声明中混合字段名和标签

### 类型说明

| .proto   | C++    | Java       | Python         | Go      | Ruby                 | C#         |
| :------- | :----- | :--------- | :------------- | :------ | :------------------- | :--------- |
| double   | double | double     | float          | float64 | Float                | double     |
| float    | float  | float      | float          | float32 | Float                | float      |
| int32    | int32  | int        | int            | int32   | Fixnum or Bignum     | int        |
| int64    | int64  | long       | ing/long[3]    | int64   | Bignum               | long       |
| uint32   | uint32 | int[1]     | int/long[3]    | uint32  | Fixnum or Bignum     | uint       |
| uint64   | uint64 | long[1]    | int/long[3]    | uint64  | Bignum               | ulong      |
| sint32   | int32  | int        | intj           | int32   | Fixnum or Bignum     | int        |
| sint64   | int64  | long       | int/long[3]    | int64   | Bignum               | long       |
| fixed32  | uint32 | int[1]     | int            | uint32  | Fixnum or Bignum     | uint       |
| fixed64  | uint64 | long[1]    | int/long[3]    | uint64  | Bignum               | ulong      |
| sfixed32 | int32  | int        | int            | int32   | Fixnum or Bignum     | int        |
| sfixed64 | int64  | long       | int/long[3]    | int64   | Bignum               | long       |
| bool     | bool   | boolean    | boolean        | bool    | TrueClass/FalseClass | bool       |
| string   | string | String     | str/unicode[4] | string  | String(UTF-8)        | string     |
| bytes    | string | ByteString | str            | []byte  | String(ASCII-8BIT)   | ByteString |

### 枚举(Enum)

```protobuf
message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
  // 定义enum类型
  // 每个enum定义的第一个元素值必须为0
  enum Corpus {
    UNIVERSAL = 0;
    WEB = 1;
    IMAGES = 2;
    LOCAL = 3;
    NEWS = 4;
    PRODUCTS = 5;
    VIDEO = 6;
  }
  Corpus corpus = 4; // 使用Corpus作为字段类型
}
```

#### 枚举别名

```protobuf
enum EnumAllowingAlias {
  option allow_alias = true; // 开启allow_alias选项
  UNKNOWN = 0;
  STARTED = 1;
  RUNNING = 1;  // RUNNING和STARTED互为别名
}
// 错误示例
enum EnumNotAllowingAlias {
  UNKNOWN = 0;
  STARTED = 1;
  RUNNING = 1;  // 未开启allow_alias选项，编译会报错
}
```

嵌套规则与 message 一致

### 注释

双斜杠注释

```go
// 我是一条注释
```

###  导入定义(imoprt)

```protobuf
import "others.proto";
```

### Map类型

proto3 支持 map 类型声明:

```protobuf
map<key_type, value_type> map_field = N;

message Project {...}
map<string, Project> projects = 1;
```

- `key_type` 类型可以是内置的标量类型(除浮点类型和 `bytes`)
- value_type 可以是除 map 以外的任意类型
- map 字段不支持 `repeated` 属性
- 不要依赖 map 类型的字段顺序

#### 关键字

- `public`

### Service

定义一个简单的 Service, `TestService` 有一个方法 `Test`, 接收一个 `Request` 参数, 返回 `Response`

```protobuf
// proto
service TestService {
	rpc Test(Request) returns (Response) {};
}

message Request {
	string name = 1;
}

message Response {
	string message = 1;
}
```

```go
// go
// 客户端接口
type TestServiceClient interface {
    // Test 测试方法
    Test(ctx context.Context, in *Request, opts ...grpc.CallOption) (*Response, error)
}
// 服务端接口
type TestServiceServer interface {
    // Test 测试方法
    Test(context.Context, *Request) (*Response, error)
}
```

### 基本规范

- 描述文件以 `.proto` 作为文件后缀, 除结构定义外的语句以分号结尾

  - 结构定义包括: message, service, enum
  - rpc 方法定义结尾的分号可有可无

- Message 命名采用大写开头驼峰命名方式, 字段命名采用小写加下划线分割方式

  ```protobuf
  message SongServerRequest {
  	string song_name = 1;
  }
  ```

- Enum 类型采用大写开头驼峰命名方式, 字段命名采用全大写加下划线分割方式

  ```pro
  enum Foo {
  	FIRST_VALUE = 0;
  	SECOND_VALUE = 1;
  }
  ```

- Service 名称与 RPC 方法名称统一采用大写开头驼峰命名方式

### 编译

```bash
protoc --go_out=. -I=. --go_opt=paths=source_relative api.proto
protoc --go-grpc_out=. -I=. --go-grpc_opt=paths=source_relative api.proto
```

#### 安装

```bash
go install google.golang.org/protobuf/cmd/protoc-gen-go
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc
```

#### 坑

- github.com/golang/protobuf/proto-gen-go 和 google.golang.org/protobuf/cmd/protoc-gen-go 是不同的, 前者是旧版本, 后者是 google 接管后的新版本, 它们之间的 API 是不同的, 也就是说用于生成的命令, 以及生成的文件都是不一样的(go-gRPC 采用的后者).

- ```bash
  protoc --go_out=plugins=grpc:. api.proto
  ```

  这种生成方式使用的就是 `github` 版的 `proto-gen-go`, 而目前这个项目已经由 google 接管了, 并且使用这种方式生成的话, 不会生成 `xxx_grpc.pb.go` 与 `xxx.pb.go` 这两个文件, 只会生成 `xxx.pb.go` 一个文件.

- ```bash
  protoc-gen-go-grpc: program not found or is not executable
  Please specify a program using absolute path or make sure the program is available in your PATH system variable
  --go-grpc_out: protoc-gen-go-grpc: Plugin failed with status code 1.
  ```

  没有安装 `protoc-gen-go-grpc` 这个插件.

- ```bash
  --go_out: protoc-gen-go: plugins are not supported; use 'protoc --go-grpc_out=...' to generate gRPC
  ```

  安装的是新版本的 `protoc-gen-go`, 但是却使用了旧版本的命令.

- 项目 module 与 proto import, 生成代码时, 路径/package 有问题

  https://segmentfault.com/a/1190000021456180













