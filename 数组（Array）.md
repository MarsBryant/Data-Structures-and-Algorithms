数组（array）是一种线性表数据结构。它用一组连续的内存空间，来存储一组具有相同类型的数据。

浅谈数组越界
分析下如下代码运行结果

```C
#include<stdio.h>
int main(int argc, char* argv[])
{
    int i = 0;
    int arr[3] = {0};
    for(i=0; i<=3; i++)
    {
        arr[i] = 0;
        printf("hello world\n");
    }
    return 0;
}
```

如果使用gcc编译后执行，输出：  
hello world  
hello world  
hello world  
hello world  
我们将i和数组的地址打印出来  

```C
#include<stdio.h>
int main (int argc, char * argv[])
{
    int i = 0;
    int arr[3] = {0};
    printf("&i= %p\n", &i);
    printf("&arr[0]= %p\n", &arr[0]);
    printf("&arr[1]= %p\n", &arr[1]);
    printf("&arr[2]= %p\n", &arr[2]);
    printf("&arr[3]= %p\n", &arr[3]);
    for (i=0; i<=3; i++)
    {   
        arr[i] = 0;
        printf("hello world\n");
    }   

    return 0;
}
```
运行结果如下：    
&i= 0x7fff134a38fc  
&arr[0]= 0x7fff134a3900  
&arr[1]= 0x7fff134a3904  
&arr[2]= 0x7fff134a3908  
&arr[3]= 0x7fff134a390c  
gcc默认情况下开启堆栈保护功能，整形变量i都会在数组之后压栈（不管i定义在数组前还是数组后），所以只会循环四次。  
我们将gcc堆栈保护功能关闭，执行上述代码  
gcc -fno-stack-protector  test.c   
运行结果：  
&i= 0x7fff48a2342c  
&arr[0]= 0x7fff48a23420  
&arr[1]= 0x7fff48a23424  
&arr[2]= 0x7fff48a23428  
&arr[3]= 0x7fff48a2342c  
hello world  
hello world  
...  
会出现死循环情况，其中arr[3]得到的是i的地址，将i重新赋值成0，从而导致死循环。

参考：https://www.ibm.com/developerworks/cn/linux/l-cn-gccstack/index.html

