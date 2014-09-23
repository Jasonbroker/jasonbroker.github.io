**深入探讨 runtime机制及KVC KVO**

上一篇文章讨论了runtime机制以及很多不错的应用，讲OC 运行时不得不提到的就是传说中的Key-Value Coding (KVC)还有Key-Value Observing (KVO)。


## Key-Value Coding (KVC) 的底层实现


之前有提到一个概念叫：

        isa指针

isa就是 is a 或者叫做is a kind of，也就是说“属于”，实际上，isa就是指向其真实类型的指针。每一个OC对象都包含此参数。

isa 奠定了oc消息机制的基础。当接受者收到消息的时候，消息函数先根据isa找到这个类，然后找到这个类的所有实现，也就是说找到了方法列表，然后试图查找要执行的方法，如果找不到就去父类中查找，直到查找到给予objc_msgSend作为参数执行或找不到这个方法则报“未识别的selector”错误。

    KVC就运用了 isa-swizzle 技术来实现查找定位的。

比如，下面的KVC代码：

        [person setValue:@"personName" forKey:@"name"];

会被编译器处理成：

			SEL sel = sel_get_uid("setValue:forKey:");

        IMP method = objc_msg_lookup (person->isa,sel);

        method(person,sel,@"personName",@"name");

其中:

SEL数据类型：它是编译器运行Objective-C里的方法的环境参数。

IMP数据类型：他其实就是一个编译器内部实现时候的函数指针。当Objective-C编译器去处理实现一个方法的时候，就会指向一个IMP对象，这个对象是C语言表述的类型。
　
KVC在调用方法setValue的时候

（1）首先根据方法名找到运行方法的时候所需要的环境参数。

（2）他会从自己isa指针结合环境参数，找到具体的方法实现的接口。

（3）再直接查找得来的具体的方法实现。

从这里看上去，一切是很美好的啊；

如果我定义了一个Person 类
		
		@interface Person : NSObject
		@property (nonatomic, readonly) int age;
readonly就是说不为age生成set方法了是吧？

然后在主函数中：

    Person *p = [[Person alloc] init];
    
    [p setValue:@"20" forKey:@"age"];
    
    NSLog(@"%d", p.age);
    
发现能够打印出值。

更为神奇的是在Person类扩展里：
		
	@interface Person()
	{
    NSString *name;
	}
	@end
	
在匿名分类中的属性是没有setter getter的。

继续在主函数中：

    [p setValue:@"20" forKey:@"age"];
    [p setValue:@"Jason" forKey:@"name"];
    
    NSLog(@"----%@,  %d", [p valueForKey:@"name"], p.age);

神奇的是：

	----Jason,  20
    
输出了结果；

如果你重写了setter方法，会发现普通的
		
		@property(nonatomic, strong)NSString *name;
		
是会调用setter和getter的！

所以在底层我想应该是KVC先寻找成员变量，如果有setter getter就调用，如果没有实现，就**直接找到该变量使用指针访问**	

	p-> name = @“Jason”;
	
这里也就实现了所有成员的获取。
    
    
    
    
    