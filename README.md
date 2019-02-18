# effective go

## 目录
[规范风格](#规范风格)

[控制结构](#控制结构)

## 规范风格

### 格式化

- gofmt作用于包层级而不是源文件层级
- go安装目录bin下gofmt
- 将gofmt放到$GOPATH/bin下，IDE保存自动format



### 注释

- Go支持C风格的块注释/**/以及C++风格的行注释 //
- //属于常规注释但是/**/大部分用在包注释



### 命名

- 首字母大小写决定了包外是否能够访问
- 只有一个方法的接口通常都是方法名加一个er
- 尽量使用大小写驼峰避免使用下划线



## 控制结构

**没有while和loop循环，只有for循环；switch更加灵活；if和switch就像for一样接受初始化声明；break和continue可以指定lable；新增select；**

```go
if init statement; condition {

}
for init; condition; post {
    
}
for condition { }
for {}
// array/slice/string/map/channel
for _, _ := range ori {}
for k := range ori {}
for _, v := range ori {}

// default no fall through
switch [] {
    case [statement|expression]:
}
```



## 函数

### 多返回值

```go
func (file *File) Write(b []byte) (n int, err error)
```

### 命名返回值

```go
func nextInt(b []byte, pos int) (value, nextPos int) {...}
```

### Defer

**方法返回前执行defer定义方法操作**



## 数据

### New分配

new：返回指针（地址），分配零值

### 构造与复合值

```go
func NewT(parammeters Type) *T {}
```

### make分配

```go
make(type, len, cap)
```

### 数组

- 数组是值拷贝，而不是地址（或指针）
- 数据类型还由数组长度决定

### Slices

### Maps

### Printing

### Append



## 初始化

### 常量

### 变量

### init方法



### 方法

#### 指针域值

方法的接收者不一定必须是结构体

```go
type byteSlice []byte
func (bs byteSlice) append(data []byte) []byte {
    return append(bs, data...)
}
func (bsp *byteSlice) append(data []byte) []byte {
    return append(*bs, data...)
}
```

**值接收者方法既可以被值和指针调用，而指针接收者只能被指针调用**



### 接口与其他类型

Golang提供了一种方式来指定对象的行为；一个类型可以实现多个接口，

#### 接口转换及类型断言

```go
type Stringer interface {
    String() string
}

var value interface{} // Value provided by caller.
switch str := value.(type) {
case string:
    return str
case Stringer:
    return str.String()
}

str, ok := value.(string)
if ok {
    fmt.Printf("string value is: %q\n", str)
} else {
    fmt.Printf("value is not a string\n")
}
```