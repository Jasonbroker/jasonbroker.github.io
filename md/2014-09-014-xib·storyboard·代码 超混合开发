**xib·storyboard·代码 超混合开发**

昨天算是朋友推荐面试，接触了一位”洁净派“iOS dev，依照他的说法来看，xib可以凑合用吧，但是storyboard是万万不可的。

有如下几点原因：
1. 无法进行团队开发，因为只要一碰storyboard就会造成代码修改，特别是在cvs以及svn 的source control下代码管理及其困难，conflict的机会非常多。（首先我作为git做src的dev，很难理解国内依然使用上世纪古董src的目的，其实是非常好迁移的。）

2. 当控制器复杂以后，各种控制器之间的跳转连线将会非常复杂，IB打开缓慢，控件移动缓慢。这个是不争的事实。

3. 只适合于轻量级开发，不适合大型项目。

4. 在项目迁移中，纯代码非常容易重用。storyboard 非常无力。

    根据他讲的，我想了一下，这些问题的确是存在的，而且看上去弃用storyboard是很有道理的。但是问题是，他并没有试图仔细寻找更好更简单的解决办法，而且对使用storyboard好像非常抵触————记得我的老开发者朋友John也说过，dev们在心里是抵触xib， storyboard的；直觉上手写代码更加geek；不到运行的时候看不到样子，一切都在我自己的掌握之中，想想可怜的美工背后指指点点的身影就能明白了；还有，在IB上拖一拖拽一拽就能搞出一个界面来？我们的饭碗岂不是要叫美工抢了？这是万万不可的。

    对于我自己，做了两年的移动开发，也是兼职UI designer；似乎并没有抢饭碗的问题存在。而且对于团队开发中所谓的conflict也没有碰到（感谢grant git）。从情怀上讲，对storyboard的以及xib的大量使用还是满欢迎的。尤其是Xcode6以后，新的IB整合，live render 简直炫酷。

    当然， 我没有与他争论优劣，对于UI的争论似乎从来没有停止过。

    因此我只是从一个不仅仅是程序员，而且是一名较有经验的UI designer的角度，谈谈这个问题的理解。

###手写代码

手写代码为学院派及众多geek推崇。是iOS UI开发最原始的方式，也是最高效的方式，可以做到在work between lines，因此只要专注代码就好了。有良好的src control，有出色的拓展性，易于重用，几乎无所不能。

他的缺点也很明显，非常慢，特别是应用在tableView和cell中的时候，xib和storyboard可以做到快手码数倍。无法直观看到UI层次结构，即使配合使用强大的Reveal（俺写过一个craker，没钱买的朋友可以拿去试用）或者RestartLessoften也不算方便access。尤其是当你使用autolayout的时候————手码简直就是噩梦。
，我相信所有做过autolayout的人都知道，唉。不多提了。

###xib

先看一个视频：https://www.youtube.com/watch?v=viLnOVBbcsE。没错里面就是Jobs。他确实是IB的忠实推荐者。苹果自家的东西大大小小，都是拿IB搞出来的，如果你仔细观察就能看出，iOS设备的系统应用都可以轻松的使用xib实现：封装的如此美妙，甚至连侧滑返回，手势添加这样小功能都能不写一句代码实现，更不要提苹果的超级布局大招autolayout了。

    当然，xib劣势也有一些，然而，随着IDE版本的迭代，多人开发代码管理问题，xml可读性太差等等问题已经得到了很大程度的完善。依然存在的问题就是代码分散。如果想要拿到某个label的属性，监听button的点击等等，会非常麻烦，拖错线不容易察觉，而且在xib中设定好的属性在代码中可以轻易的被覆盖，只从xib中看无法察觉此错误。

###storyboard

使用storyboard快速创建应用，非常适合互联网环境下的小型团队敏捷开发的需要。
自从Xcode5 以后，就不能建立空项目了，默认为单视图控制器。对storyboard的不断加强，说明苹果始终把storyboard作为大方向，就像是将mrc向arc过度一样。开发者应该感谢苹果，他们的始终为减少开发者工作不断的努力着。（笑看android dev）。

storyboard的缺点，确实是存在多人开发的conflict问题。解决方案也很简单，就是混合超敏捷开发。所谓超敏捷，其实是指个人层面的敏捷开发，也就是在部署任务后，能够用最简单最快捷的方法在保证质量的前提下实现任务，提升效率。

那么唯一一个问题就是性能了，之前学iOS时，记得看过一本书讲性能优化，专门说xib性能优于storyboard，最好开发少用storyboard。现在想想，这么多底层api性能这么高你咋不调用呢？不就是因为方便嘛？而且实际上，我并没有看出拿一点点加载缓慢对UE有什么影响。



###混合UI搭建超敏捷开发

这个概念应该是我在加州大学念书的时候做项目摸索出来的，在美国用的人不多，在中国不知道在dev里面是否有人在用。

所谓混合UI搭建超敏捷开发，是适用于几乎所有类型iOS项目中，尤其是适合于需要快速上线并短周期迭代的互联网类应用。

所谓混合UI搭建，就是多storyboard搭建小型跳转界面，多xib进行控件描绘，代码进行多storyboard联系粘合。

一个非常典型的混合超敏捷的例子是TableView 静态表单的创建。
一般来说，对于不同种类的cell需要自定义cell类，创建cell模型，将相应数据模型填充，然后显示在controller上面。 从封装角度来说，这是非常正确的，但是操作起来却非常繁琐，不论是创建cell模型，还是数据模型，以及提供数据源都是非常烦的。
storyboard创建静态表单简直逆天的简单，不出10分钟，就可以搞定一个简单的setting界面，根据接口实现里面所有功能。

当然对于多人开发而言，不论项目是否存在storyboard，我们都可以手动的快速添加storyboard接口。

        UIStoryboard *board = [UIStoryboard storyboardWithName: @"ZCSettingStoryboard" bundle:[NSBundle mainBundle]];
        ZCSettingViewController *settingVc = [board instantiateViewControllerWithIdentifier: @"settingVC"];

        settingVc.title = @"设置";
        settingVc.view.backgroundColor = [UIColor whiteColor];
        //跳转
        [self.navigationController pushViewController:settingVc animated:YES];

在触发跳转的控制器中，只需要加入3行代码就可以快速实现这个界面接口。
1. 从storyboard文件中加载storyboard；
2. 拿到接口控制器；
3. 执行跳转

没有做这个功能的member永远不会打开这个storyboard，所以代码冲突也没什么可担心的。
我自己在团队里使用没有geek提出异议来————因为很可能他们根本不了解我如何实现这个segment，当然这根本与他们无关，而且老板也觉得我的干活很快，代码简洁有力——如果你的代码都是在处理核心事务，相比你自己都觉得好爽啊。而且你会惊奇的发现，storyboard也可以重用了！。试着让你的storyboard模块化，在开发中可以事半功倍。

遇到自定义控件？

如果是可视的，使用自定义xib不是一件坏事。良好的封装也可以使得xib方便重用。典型的例子是使用xib自定义cell或者tabbar，searchbar，segment control等等控件。

对于充满丰富视觉效果的自定义控件，例如graph，bar，hud等等，还是需要强大的quarz2D来实现的，当然苹果官方推荐尽量不要自定义控件，除非万不得已。既然是敏捷开发，尽量使用系统自带控件也能加快开发进度，而且，github里面优秀的三方framework非常多，借鉴下也是不错的选择。

OK，随着Xcode的升级，对新的autolayout sizeClass的引入，多种屏幕的适配将更加紧迫的要求我们减少垃圾UI代码，让程序员更加专注于核心业务的实现。在这里，我也抛砖引玉讲了自己在团队中一直使用的协作方法，希望能有所收获。










