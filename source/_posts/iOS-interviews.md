---
title: iOS面试相关
date: 2016-12-23 10:46:43
tags: 
- iOS
- 面试
---

面试题总结。

#### [招聘一个靠谱的iOS](https://github.com/ChenYilong/iOSInterviewQuestions/blob/master/01《招聘一个靠谱的iOS》面试题参考答案/《招聘一个靠谱的iOS》面试题参考答案（上）.md#1-风格纠错题)

1. 什么情况使用 weak 关键字，相比 assign 有什么不同？
	
	* 什么情况使用 weak 关键字？
	
   1. 在 ARC 中,在有可能出现循环引用的时候,往往要通过让其中一端使用 weak 来解决,比如: delegate 代理属性
	2. 自身已经对它进行一次强引用,没有必要再强引用一次,此时也会使用 weak,自定义 IBOutlet 控件属性一般也使用 weak；当然，也可以使用strong。

   * 不同点：
   
	1. weak 此特质表明该属性定义了一种“非拥有关系” (nonowning relationship)。为这种属性设置新值时，设置方法既不保留新值，也不释放旧值。此特质同assign类似， 然而在属性所指的对象遭到摧毁时，属性值也会清空(nil out)。 而 assign 的“设置方法”只会执行针对“纯量类型” (scalar type，例如 CGFloat 或 NSlnteger 等)的简单赋值操作。
	2. assign 可以用非 OC 对象,而 weak 必须用于 OC 对象

2.  IBOutlet连出来的视图属性设置成weak or strong?
	除了UIViewController的View等被File's Owner直接拥有的对象应该用strong，其他一般用weak。
	
3. 这个写法会出什么问题： @property (copy) NSMutableArray *array;
	
	* 添加,删除,修改数组内的元素的时候,程序会因为找不到对应的方法而崩溃.因为 copy 就是复制一个不可变 NSArray 的对象;
	* 使用了 atomic 属性会严重影响性能；
	该属性使用了同步锁，会在创建时生成一些额外的代码用于帮助编写多线程程序，这会带来性能问题，通过声明 nonatomic 可以节省这些虽然很小但是不必要额外开销。
	
4. 