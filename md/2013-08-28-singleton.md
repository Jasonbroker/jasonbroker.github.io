##单例设计模式(Singleton)
单例模式是非常重要的设计模式，它可以保证某个类创建出来的对象永远只有1个。
他的应用场合也非常多

2> 作用(为什么要用)
* 节省内存开销
* 如果有一些数据, 整个程序中都用得上, 只需要使用同一份资源(保证大家访问的数据是相同的,一致的)
* 一般来说, 工具类设计为单例模式比较合适

系统中的单例：常见的单例有
        [UIapplication sharedApplication];
        [NSFileManager defaultManager];
        [NSNotificationCenter defaultCenter];
        [NSUserDefaults standardUserDefaults];

等等。

        通常，这些对象在应用程序实例中只需要创建一份。

##单例的简单创建

一个完整的单例创建：

可以使用该单例宏：**<    >**

// .m
        #if __has_feature(objc_arc) // is ARC

        static id _instance = nil;
        + (id)allocWithZone:(struct _NSZone *)zone
        {
            if (_instance == nil) {
            static dispatch_once_t onceToken;

                dispatch_once(&onceToken, ^{
                _instance = [super allocWithZone:zone];
                });
            }

        return _instance;

        } 

        - (id)init 
        { 
        static dispatch_once_t onceToken; 
            dispatch_once(&onceToken, ^{
            _instance = [super init];
            });
        return _instance;
        } 

        + (instancetype)sharedClass
        { 
            return [[self alloc] init];
        }

        + (id)copyWithZone:(struct _NSZone *)zone 
        { 
            return _instance;
        } 

        + (id)mutableCopyWithZone:(struct _NSZone *)zone 
        { 
            return _instance;
        }

        #else // is mrc

        static id _instance = nil;
        + (id)allocWithZone:(struct _NSZone *)zone 
        { 
            if (_instance == nil) {
            static dispatch_once_t onceToken;

            dispatch_once(&onceToken, ^{
            _instance = [super allocWithZone:zone];
            });
        } 
        return _instance;
        } 

        - (id)init 
        { 
            static dispatch_once_t onceToken;
            dispatch_once(&onceToken, ^{
            _instance = [super init];
            });
            return _instance;
        } 

        + (instancetype)sharedClass
        { 
            return [[self alloc] init];
        } 

        - (oneway void)release 
        { 

        } 

        - (id)retain 
        { 
            return self;
        } 

        - (NSUInteger)retainCount 
        { 
            return 1;
        }

        + (id)copyWithZone:(struct _NSZone *)zone 
        { 
            return _instance;
        } 

        + (id)mutableCopyWithZone:(struct _NSZone *)zone 
        { 
            return _instance;
        }

        #endif



