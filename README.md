# MySampleApp

https://www.zhihu.com/question/35009721
作者：知乎用户
链接：https://www.zhihu.com/question/35009721/answer/60868492
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

项目工程搭建App工程结构搭建：几种常见Android代码架构分析 总结了一些关于项目工程搭建的范例。我们在搭建工程结构的时候可以尽量抽取一些共用的东西，例如，数据库操作、base、task、事件观察者、通用的工具类、UI公共组件等等，这些东西应该表现在代码结构中。&lt;img src="https://pic4.zhimg.com/574c41a5a8a5cd1314c8e86253d7be1b_b.png" data-rawwidth="399" data-rawheight="415" class="content_image" width="399"&gt;这些包名的作用一目了然，在别人接手这个项目的时候就会相对简单。这些包名的作用一目了然，在别人接手这个项目的时候就会相对简单。adapter 适配器，如果业务复杂，根据不同的业务可以添加子包来进行分类；base 用来存放View的基类，例如BaseAcitivity、BaseFragment，甚至可以添加某些不同actionbar主题的Base类；common 当然是存放一些共用的配置类信息，常量等等；controller 控制器，将一部分的业务类需求放到里面，充当db和View交互的中间层，减少Activity中业务的复杂性；db数据库类event 观察者模式，事件通知；task一些AsyncTask任务类view一些自定义组件vo 值对象，其实就是给各个组件使用的对象，比如ListView的Item对象等等widget UI界面AppContext 自定义Application类另外，根据自己的一些业务需求，我们可能需要单独的抽取一些核心的包类。比如，理财类软件在搭建工程结构的时候，可以单独抽出了2个JS相关的核心包类：&lt;img src="https://pic3.zhimg.com/4ea4362d6800915bbf6fbcad3c9c9f22_b.png" data-rawwidth="281" data-rawheight="47" class="content_image" width="281"&gt;2.AppContext 的处理    Application本身在一个应用中只会存在一个实例，所以它一般用来存储一些全局的变量和一些只需要处理一次的数据。context的管理。这个和BaseActivity组合使用，将每一个Activity放到一个列表中，需要的时候直接使用即可；&lt;img src="https://pic4.zhimg.com/53014f9d58df74fd60c891ec9d643dd3_b.png" data-rawwidth="908" data-rawheight="38" class="origin_image zh-lightbox-thumb" width="908" data-original="https://pic4.zhimg.com/53014f9d58df74fd60c891ec9d643dd3_r.png"&gt;&lt;img src="https://pic3.zhimg.com/4e0eb5c528105b061d40b5790c52e3c6_b.png" data-rawwidth="820" data-rawheight="129" class="origin_image zh-lightbox-thumb" width="820" data-original="https://pic3.zhimg.com/4e0eb5c528105b061d40b5790c52e3c6_r.png"&gt;初始化和记录一些app信息，例如app的版本信息、设备信息等等；初始化特定的业务需求，例如有盟统计类、分享SDK、推送等等记录应用启动次数、是否第一次安装等等，如果在第一个版本不加，到后面版本使用次记录会很麻烦（血泪教训……）记录是否开启处于调试模式。   在输出日志、错误消息的时候有用。 public final static boolean DEBUG=BuildConfig.DEBUG;
3.Base的处理    对BaseActivity的处理好坏一定程度上会影响项目的代码可读性，在Base里面做一些规范化处理将会大大减少代码的书写量和提高可读性。将其Base类定义成抽象类，增加一些抽象方法，例如findView的处理、onClick的处理、初始化数据的处理。例如可以重载setContentView方法来规范子类的行为：    @Override
    public void setContentView(int layoutResID) {
        super.setContentView(layoutResID);
        findView();
        initView();
        setOnClick();
    }
   /**
     * 获取布局控件
     */
    protected abstract void findView();

    /**
     * 初始化View的一些数据
     */
    protected abstract void initView();

    /**
     * 设置点击监听
     */
    protected abstract void setOnClick();
通过这种规范可以大大减少后期代码的混乱，onCreat方法中存在大量杂乱无章的代码;添加观察者模式的支持。具体的可以看我的博客观察者模式在android 上的最佳实践定义一些ActionBar上面的保护类方法，比如返回按钮、下拉事件等等;4.数据库的处理 个人建议在处理数据库的时候采用ContentProvider的方式，有2个优点：采用URI的方式访问，更加符合我们的使用习惯；随时可以提供给其它应用访问数据库；5.图片的处理    对图片处理的文章很多，其实你只要把基本的一些开源框架原理搞清楚，对普通应用其实足够了。最近在做一个开源项目daliyan/MyBlog · GitHub 需要的可以参考参考，期待大牛的回答。



