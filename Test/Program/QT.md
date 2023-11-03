- 对象树
解决基类Object派生的子类使用new创建后不需要使用delete进行释放
创建children talble，析构Object时全部子类都会进行释放
所有的子类都派生于Object 使用new创建在堆区可以自动释放，简化内存回收机制
顶层窗口 窗口对象继承与Qwidget 不使用new程序管理 直接放入栈 中程序管理
- 信号和槽函数
信号发送者和接受者解耦合（松散耦合），通过connect连接二者
signal： 只能是void 只用声明不用实现
slots：QT5之前 slots必须声明为public slots。 QT5之后可以随便写，因为connect不会用到slots，当做成员函数使用

connect绑定地址
信号+信号 信号+槽函数
1.signal+signal 2. signal+multi slots 3.multi signal + slots 4.signal和slot参数必须对应
5.signal slots参数个数可以不同 signal个数可以大于slots个数 槽函数选择性接收但类型对应
mutable 可修改值传递参数的拷贝，不改动本身
利用lambda规避signal参数大于slots参数的限制 实现更多功能

- 资源文件
添加QT资源文件，并在工程下添加资源文件夹并编译
- 对话框
1. 模态
2. 非模态
3. 消息对话框
4. 标准对话框
- 事件 Event
重写事件函数实现功能（mousePressEvent/mouseReleaseEvent/mouseMoveEvent)
事件分发器：对本widget中的event(QEvent * e)重写 使用e->type()判断事件 ture不分发实现事件拦截
事件过滤器：event上一级的拦截，在主widget给widget添加过滤器installEventFilter(this)并重写eventFilter(QObject * obj, QEvent * e)
- 定时器 timer
1. 主Widget绑定的int id = startTimer(100) ms
2. QTimer * timer = new QTimer(this)
- 绘图 Painter
  *非磁盘保存类绘图必须重写paintEvent（QPaintEvent *)*
 1. Widget绘图 QPainter p(this);
 2. PaintDivice : QPixmap QImage(像素级操作) QPicture（记录和重现绘图命令） 

> 1. 使用指针显示widget必须添加window类属性 QWidget不能直接显示 但是最顶层widget可以
> 2. foodName.toUtf8().data(); //QString 转 QByteArray 转 char*

