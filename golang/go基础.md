# go基础
## 1、第一个go程序
```**package main

import (
	"fmt"
    "time"         
)

func goFunc(i int){
     fmt.Println("goroutine",i,".....");
}

func main()  {
	for i := 0; i < 10000; i++ {
		go goFunc(i) //开启一个并发协程
	}

	time.Sleep(time.Second)
}**
```
## 2、常见的变量声名方式
```package main

import(
	"fmt"
)

func main(){

	//1、声名一个变量，默认值是0
   var a int	
   fmt.Println("a=",a)

   //2、声名一个变量，并初始化
   var b  int =100
   fmt.Println("b=",b)

   //3、在初始化的时候，可以省去数据类型，通过值自动匹配当前变量的数据类型
   var c=200
   fmt.Println("c=",c)
   fmt.Printf("type of c=%T\n",c)

   //4、省去var关键字，直接根据值自动匹配类型,该方法不支持全局变量声名
   d:=900
   fmt.Println("d=",d)
   fmt.Printf("type of d=%T\n",d)

   e:="abcd"
   fmt.Println("e",e)
   fmt.Printf("type of e=%T\n",e)

   //多个变量的声名
   var f,g int=99,98
   fmt.Println("f=",f,"g=",g)

   var h,i=78,"abcdefg"
   fmt.Println("h=",h,"i=",i)

   var(
	 vv int=10
	 jj bool=true
   )
   fmt.Println("vv=",vv,"jj=",jj)

}
```
![](vx_images/154071816236448.png =408x)
 