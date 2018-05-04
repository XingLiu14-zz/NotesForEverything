Golang
======

### [A Tour of GO](https://tour.golang.org/welcome/1)
#### Package, variables and functions
- Every GO program is made up for package, usually the package imports into a parenthesized, "factored" import statement（包名和导入路径的最后一个元素一致）:
```
import (
	"fmt"
	"math"
)
```
- You can also write multiple import statements like:
```
import "fmt"
import "math"
```
- 对于一个包，名字以大写开头代表已导出可以用，小写开头代表未导出`math.Pi`可以用，`math.pi`不能用。
- Go的参数类型在函数名和参数名后面，当连续两个及以上已命名形参类型相同时，可只写最后一个类型。
```
x int, y int
=>
x, y int
```
- 函数可以返回多个值。返回值可以被命名并被视作定义在函数顶部的变量，返回值名称应有意义作为文档使用，用`return`直接返回，仅应用于短函数中，否则会影响代码可读性：
```
func split(sum int) (x, y int) {
	x = sum * 4 / 9
	y = sum - x
	return
}
```
- 用`var`关键字定义变量，可在函数或包级别，类型在变量名后。若初始化值已存在，可省略类型。可用`:=`代替`var`申明，但是因为函数外所有语句必须以关键字开头，所以只能在函数里用：
```
var i, j int = 1, 2

func main() {
	k := 3
	c, python, java := true, false, "no!"

	fmt.Println(i, j, k, c, python, java)
}
```
- Go的基本类型（没有标记长度的在32位机器上就是32位宽，在64位机器上就是64位宽）：
```
bool
string
int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr
byte // uint8的别名
rune // int32的别名，表示一个 Unicode 码点
float32 float64
complex64 complex128
```
- 当没有赋初始值的时候，变量自动赋0值。数值类型为`0`，布尔型为`false`，字符串为`""（空字符串）`
- T(v)表达式将值v转换为类型T，与C不同的是，必须要显示转换
- 在声明一个变量而不指定类型时，变量类型由右值导出，若右值是未指明类型的数值常量，类型要取决于常量精度
- 声明常量用`const`而不是`var`，常量不能用`:=`声明

#### 流程控制语句
- Go只有`for`循环，for语句后没小括号，有大括号，三个语句用`;`分开
	+ 初始化语句（optional）：在第一次迭代前执行
	+ 条件表达式：在每次迭代前求值
	+ 后置语句（optional）：在每次迭代的结尾执行
- 当初始化语句和后置语句都没有时，去掉所有分号，我们发现for变成了`while`！
- `if`也不需要小括号，但是需要大括号。if前面竟然也能加一个初始化语句，和条件表达式用`;`隔开
- `switch`：运行第一个值等于条件表达式的case语句，可以用`fallthrough`让它继续执行。case无需为常量，且取值不必为整数
```
switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.", os)
	}
```
- 没条件的switch相当于一连串的if-then-else
- `defer`语句可以把函数推迟到外层函数返回后执行，其中参数会立即求值，FILO

#### struct, slice and maps
- 指针：`*T`是指向T类型的指针，零值为`nil`；`&i`去指针底层值。但是Go没有指针运算
- `Struct`：用点号来访问字段，也可以用指针（隐形访问使指针表面上和struct一样），还可以用struct literals的`Name:`语法来指定部分字段
```
type Vertex struct {
	X int
	Y int
}

var (
	v1 = Vertex{1, 2}  // has type Vertex
	v2 = Vertex{X: 1}  // Y:0 is implicit
	v3 = Vertex{}      // X:0 and Y:0
	p  = &Vertex{1, 2} // has type *Vertex
)

func main() {
	v := Vertex{1, 2}
	v.X = 1e4
	p := &v
	p.X = 1e9
}
```





