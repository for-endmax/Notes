# Golang ACM模式要点

## 输入

```go
// 逐个读取
fmt.Scan()  
fmt.Scanf()
fmt.Scanln()

// 整行读取
input:= bufio.NewScanner(os.Stdin)
input.Scan() // 读取一行内容
input.Text() // 获取字符串
```

## 输出

```go
fmt.Println()
fmt.Printf()
// 小数位数
fmt.Printf("%.2f",a)
```

## 字符串操作

```go
// 字符串切分为slice
strings.Split(s," ")
// 字符串拼接
var buffer bytes.Buffer
for i := 0; i <= 9; i++ {
    buffer.WriteString(strconv.Itoa(i))
}
fmt.Println(buffer.String())
// 字符串替换
strings.Replace("ABAACEDF", "A", "a", 2)  // aBaACEDF
// 字符串转大写
strings.ToUpper("abaacedf")
// 字符串转小写
strings.ToLower("abaacedf")
// 去除首尾字符
strings.Trim()
strings.TrimRight()
strings.TrimLeft()
```

## 切片操作

```go
// 复制
copy（dst,src）
// 转换为字符串
strings.Join(slicce,",")
```

