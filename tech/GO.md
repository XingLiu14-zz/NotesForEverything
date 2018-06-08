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
- 数组：初始化`primes := [6]int{2, 3, 5, 7, 11, 13}`，数组大小是固定的
- slice（切片）是一个动态大小的数组，`[]T`代表是元素类型为T的切片。`a[low : high]`界定一个半开区间，包含第一个元素不含最后一个元素
- 切片并不会储存数据，更改切片元素会修改底层数组中对应的元素，共享底层数组的其他切片也会观测到修改
- `[]bool{true, true, false}`会先创建一个数组，再构建一个引用它的切片
```
q := []int{2, 3, 5, 7, 11, 13}

r := []bool{true, false, true, true, false, true}

s := []struct {
	i int
	b bool
}{
	{2, true},
	{3, false},
	{5, true},
	{7, true},
	{11, false},
	{13, true},
}
```
- 切片上界默认是0，下界默认是切片长度
- 切片的长度`len(s)`和容量`cap(s)`。长度是里面元素的个数，容量是到其底层数组元素末尾的个数，所以长度永远小于等于容量
- 切片的零值是nil，长度和容量都是0且没有底层数组
- 可以用`make`函数创建切片，第二个参数是len，第三个参数是cap
```
a := make([]int, 5)
b := make([]int, 0, 5)
c := b[:2]
d := c[2:5]
```
- `func append(s []T, vs ...T) []T`，第一个参数是一个元素类型为T的切片，后面是T的值。当底层数组太小时，会自动分配它会自动更大数组（大小不一定会匹配正好要的）
- for循环的`range`形式可以遍历切片或map，每次都会返回两个值，第一个为当前下标，第二个为对应元素副本。若想忽略下标，赋`_`即可。
- **Practice: Slcie**
```
package main

import "golang.org/x/tour/pic"

func Pic(dx, dy int) [][]uint8 {
	p := make([][]uint8, dy)
	for i := range p {
		p[i] = make([]uint8, dx)
	}
	
	for y, row := range p {
		for x := range row {
			row[x] = uint8(x%(y+1))
		}
	}
	return p
}

func main() {
	pic.Show(Pic)
}
```
- `Map`
```
var m = map[string]Vertex{
	"Bell Labs": {40.68433, -74.39967},
	"Google":    {37.42202, -122.08408},
}

m[key] = elem
elem = m[key]
delete(m, key)
elem, ok = m[key]
```
- 函数可以当函数的参数，传一个函数进去。闭包，数据与行为的结合
- struct的method，如果要修改struct的值，则需要用指针当接受者。并且这种情况下，传值也可以
```
func (v *Vertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
```
- Interface
```
type Abser interface {
	Abs() float64
}

func main() {
	var a Abser
	f := MyFloat(-math.Sqrt2)
	v := Vertex{3, 4}

	a = f  // a MyFloat 实现了 Abser
	a = &v // a *Vertex 实现了 Abser
}

type MyFloat float64

func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

type Vertex struct {
	X, Y float64
}

func (v *Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```






























