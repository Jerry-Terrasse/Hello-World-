测试环境：Linux系统，使用time命令测量user time，测量3至4次取平均值（事实上，每次的误差已经相当小）

# 1. inline 和 register

- ## register 效率测试

```cpp
//Program 1
#define MAX 1000000000
int main()
{
	for(int i=1;i<=MAX;i++);
	return 0;
}
```

以上程序进行`1e9`规模的循环，运行时间为`2.416`秒。

若将第5行改为`for(register int i=1;i<=MAX;i++);`，时间将暴降至`0.320`秒，快了接近8倍。

这说明`register`的优化幅度是相当大的，在循环变量里应该多加使用。

----

```cpp
//Program 2
#define MAX 1000000000
int main()
{
	for(int i[30000]={1};i[29999]<=MAX;i[29999]++);
	return 0;
}
```

以上程序仍然进行`1e9`规模的循环，运行时间为`1.887`秒。快于单个变量的循环。

但是再加上`register`后运行效果并没有变化，这说明`register`对于内存较大的变量就没有效果了。

不同机子的寄存器大小不一样，`register`的上限也不一样，不过据说在洛谷是比较大的。

- ## inline 效率测试

