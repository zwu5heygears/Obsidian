坚持转换到3D数据处理（Open3D/PCL）（点云切片）
坚持复习使用深度学习和强化学习理论知识解决现有问题 PYTHON+PYTORCH

# <font size = 10 color = "yellow">7.24</font>
## 安装git
```bash
git config --global --list
git config --global user.name "zwu5heygears"
git config --global user.email "zwu5@heygers.com"
git init
git clone
git add file
git commit -m “测试信息”
git push (密码需要一个权限足够大的toke）
git pull
git reset
git diff
git status
git log
git branch 查看分支
git branch b1
git branch -d/-D b1
git merge (未修改的分支）
git remote add url
git remote -v
git remote remove url
```
## 了解AutoCalib代码流程
Ubuntu安装中文输入法
不要安装搜狗输入法，直接安装IBUS
qt无法使用中文，需要设置环境变量 
## 服务器连接
sftp  [heygears@10.99.20.191](mailto:heygears@10.99.20.191)
密码：heygears008
命令：[https://blog.csdn.net/u014297502/article/details/126928286](https://blog.csdn.net/u014297502/article/details/126928286)
开始使用obsidian做知识梳理和项目规划
确定编译平台、安装包依赖、以及开发工具

# <font size = 10 color = "yellow">7.25</font>

## 1. 多线程
线程使用ref（）传递引用时不要对其进行操作，使用临时变量赋值出即可
```cpp
thread th1 = thread(func);
thread th2 = thread(func2, p1)  // 创建
thread th3 = thread(func3, ref(p2)) // bind和thread传递引用需要ref包装
th1.joinable() // 是否可接入
th1.join() // 阻塞运行
th1.detach() // 放弃接管



std::future() // 获取异步线程的结果，结果也是异步的
```

## 2. move+callback function

## 3. binds
绑定函数指针（进行回调）(回调注册：绑定相应的函数接口)
```cpp
	auto f1 = bind(fun_1, 1, 2, 3);
	auto f2 = bind(fun_1, placeholders::_1, placeholders::_2, 3);
	auto f3 = std::bind(fun_1, placeholders::_2, placeholders::_1, 3);
	auto f4 = std::bind(fun_2, placeholders::_1, n);
	cout << "m=" << m << "n=" << n << endl; 
	// 使用bind绑定是值传递（忽略&），不预先绑定（占位符）引用（原逻辑）
	auto f5 = std::bind(&A::fun_3, a, placeholders::_2, placeholders::_1); 
	// class成员函数需要传入指针引用
	std::function<void(int, int)> fc = std::bind(&A::fun_3, a, std::placeholders::_1, std::placeholders::_2);
```
5.unique_lock shared_mutex  lock
```cpp
共享锁？？
```
https://blog.csdn.net/qq_42604176/article/details/120927167
6.share_ptr unique_ptr weak_ptr


7.boost::asio::thread_pool



8.PID
P：目标偏差error乘以系数K-> $K_p * e(t)$
I：误差在时间上的积分->$K_i\int_0^ t e(\tau)dr$
D：误差变化曲线的导数

9.Template  模板编程
10.std::chrono duration
# <font size = 10 color = "yellow">7.26</font>

## 1.OPENCV
circle函数
插值算法（最近邻差值算法、双线性差值算法（默认）、双三次差值算法）用在图像缩放放大中（resize等）

## 2. Boost.Log  仍在继续


## 3. 配置Bosot
官网下载后安装https://blog.csdn.net/yylxiaobai/article/details/127558967https://blog.csdn.net/yylxiaobai/article/details/127558967


## 4. 复写AutoCalib代码


## 5. cpp 反射机制 访问Struct 成员

https://www.zhihu.com/question/598203489/answer/3004292303?utm_id=0
什么叫反射？为什么for（auto）需要有begin（迭代器）？使用boost进行结构体访问？
```cpp
#include <boost/fusion/adapted/struct.hpp>
#include <boost/fusion/include/for_each.hpp>
struct PrintA {
	template<typename T>
	void operator()(const T& filed)const {
	std:cout << field << " ";
	}
};

BOOST_FUSION_ADAPT_STRUCT(
boostStruct,
	(int, x)
	(int, y)
	(int, z))
int main(){
	boostStruct a1{1, 2, 3};
	boost::fusion::for_each(a1, PrintA);
	return 0;
}
```


# <font size = 10 color = "yellow">7.27</font>
## 1. 继续复写自动校准代码并优化
vetorc.clean() 的好处是clear掉再push/emplace_back比赋值效率高太多当然不排除现代[编译器](https://www.zhihu.com/search?q=%E7%BC%96%E8%AF%91%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A2774824565%7D)能够识别并优化掉这种愚蠢的行为）。在c++中要始终牢记：不必要的内存申请释放和复制是百分之九十以上性能问题的来源。

## 2. STL容器使用规范化（Vector）


## 3. const &
按引用（而不是按值）传递对象可以节省内存和时间，尤其对于大型对象。按值传递对象的主要优点在于可以保护原始数据，但可以通过将引用作为const类型传递，来达到同样的目的
Mat传输形参，值传递是传指针会进行修改，如果是浅拷贝赋值不会带出（局部变量销毁），cosnt不可修改
# <font size = 10 color = "yellow">7.28</font>
## 1. 结束AutoCalib的重构并解决全部问题

要记住，不会的东西永远都有，该学会什么才最重要，什么才是优先学习的。

Mat的深拷贝支出现在copyTo和clone，&中，其他都为浅拷贝，都是只复制指针

vector使用at比[ ]访问要更安全，[ ]不检查是否超出索引，效率更快
> 错误需要全部传递给log查看，Warning（） Debug（） Error（）info()
> Mat的函数传递最好都用&引用，效率更高，防止改变用const &

# <font size = 10 color = "yellow">7.31</font>
## 1. 排查AutoCalib全部bug
错误码只有在面向外部接口（非本人）代码才需要POST

## 2. CallBack


## 3. bind

## 4. Doxygen


## 5. VNC

## 6. 重复学习git
TODO CLIon版本的git使用和DOxygen的使用

# <font size = 10 color = "yellow">8.1</font>

## 1.CMAKE

<font color = #8B6CEF size = 6>创新</font>
是否可以使用凸优化等方式加快光源校准，而不是简单的灰度加减
Blob生成函数无需仿射变换，circle函数可以画到画幅之外

# 类型转换
```cpp
char + int   char转ASCII
int + double int转double

int(a)
float(a)
double(a)
to_string(a)

char - '0'  char转int
int + '0' int转char
static_cast<type>()
memcpy() 
union() 转换

std::vector<char> floatToByteArr(const float& data)
{
    std::vector<char> byteVec;
    char* p = (char*)&data + sizeof(float);
    for (size_t i = 0; i < sizeof(float); ++i)
    {
        byteVec.push_back(*(--p));
    }
    return byteVec;
}
