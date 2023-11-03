# 基础类型
```cpp
char unsigned char char16_t char32_t wchar_t 1 1 2 4 2(4)
short ushort 2 2
int long    long long 4 (4)8 8
float 4
double 8
size_t 4(32)8(64)
bool 1
```

```cpp

# STL
标准模板库
从逻辑层次来看，在STL中体现了泛型化程序设计的思想。
在这种思想里，大部分基本算法被抽象，被泛化，独立于与之对应的数据结构，用于以相同或相近的方式处理各种不同情形
从实现层次看，整个STL是以一种类型参数化（基于模板）的方式实现的
1. 容器 各种数据结构 
2. 适配器 改变容器结构的组件 安全简化的接口类
3. 算法 sort find等
4. 迭代器  连接算法和容器
5. 函数对象
6. 分配器

```cpp
序列式容器：
1. vector
2. deque
3. list
5. stack
关联式容器：
6. set
7. multiset
8. map
9. multimap
10. unordered_set
11. unordered_map
12. unordered_multiset
13. unordered_multimap


// vector 顺序 底层数组 访问-插入-查找 O(1) O(n) O(n)
vector<int> v1;
vector<int> v2{1, 2, 3, 4, 5};
vector<int> v3(v1.begin() + 1, v1.end());
v1.at(2) v1[2]
v1.front()
v1.back()
v1.begin() cbegin()
v1.end() cend()
v1.rbegin() crbegin()
v1.rend() crend()
v1.insert(v1.begin() - 1, 1); 
v1.insert(v1.end(), 3, 5); 
v1.insert(v1.end(), v2.begin(), v2.end())
v1.insert(v1.end(), {1, 3, 5})
v1.emplace(v1.begin(), 2) // 单个指定位置，效率高, 只能插入单个
v1.push_back(1);
v1.emplace_back(1);
v1.pop_back();
v1.size()
v1.empty()
v1.capacity() v1.reserve(size)
v1.clear()

v1.erase(v2.begin()) 
V1.erase(v2.begin(), v2.end() - 1) // 迭代器失效，返回下一个迭代器

remove(v1.begin(), v1.end(), 12)// 不改变capacity size 迭代器指向下一个元素（相同） algorithm
v1.assign(7, 6) v1.assign(v2.begin(), v2.end()) // 修改所有元素
fill(v1.begin(), v1.end()-2, 10) // 修改指定范围元素
reverse(v1.begin(), v1.end())
v2.swap(v1)
// # vector.end()指向的是最后一个元素的下一个位置，所以访问最后一个元素的正确操作为：vector.end() - 1;
vector<int>::iterator = find(v1.begin(), v1.end(), 20);
    auto it1 = find(v2.begin(), v2.end(), 1);
    if(it1 == v2.end())
    {
        cout << "not find";
    }
    else{
        cout << distance(v2.begin(), it1);  // 迭代器距离返回
    }
    
vector<int>::size_type = count(v1.begin(), v1.end(), 2);
vecotr<int>::size_type = count_if(v1.begin(), v1.end(), Fun);
for(auto &i : v1) cout << i;
for(auto i = v1.begin(); i ! = v1.end; ++i){cout << *i}


/// list 链式 底层双向链表 访问-插入删除-查找 O(n) O(1) O(n)
list<int> list1;
list<int> l2{1, 2, 3, 4, 5};
list1.push_back()
list1.push_front()
list1.remove(3)
find()
pop_back()
pop_front()
insert()
int a = *l2.begin()
int b = *(l2.begin()+2) // 不可行
int c = *(l2.begin()++) // 可行 重载过
for(auto i = l2.begin(); i ！= l2.end(); ++i){cout << *i;}
for(auto &i : l2){cout << i;}
l2.merge(l1) // 合并链表
l2.clear();

// deque 顺序 list和vector的组合(中央控制区+多个缓冲区) 访问-插入-查找 O(1) O(n) O(n) 支持at [] 顺序访问
deque<int> deque1;
push_back()
pop_front()
deque1.at(0) deque1[0]

// stack 底层list or deque 封闭头部
stack<int> stk1;
stack<int> stk2(stk1);
deque<int> deq1{1, 2, 3, 5, 5};
stack<int> stk3(deq1); // stack不可用初始化列表，可用其他容器初始化
stk1.push(1);
stk1.pop(1);
auto v = stk1.top();


// map 红黑树 自动排序
map<int, float> m1;
map<int, float> m1(m2):
map<int, float> m1(m2.begin(), m2.end() - 1);
pair<int, float> p1;
m1.insert(make_pair(1,1.0));
m1.emplace(p1);
m1.erase(key) // diff 可以直接删除指定的Key
m1.erase(m1.begin())
m1.find(3) or find(m1.begin(), m1.end(), 3)
v = m1[key].first // no .at
for(auto i = m1.begin(); i ! = m1.end(); ++i){
cout << i- > first << i- > second << " ";
}
map<char, int>::iterator itlow, itup;
itlow = map1.lower_bound('b');     // itlow points to b  !!!!同时用来搭配访问multimap
itup = map1.upper_bound('d');      // itup points to e
map1.erase(itlow, itup);                    // 剩下a 和 e
pair<map<int, int >::iterator, map<int, int>::iterator> ret;
ret = map22.equal_range(1);  // 直接返回lower/upper bound

// multimap key可以重复 因此不能访问key-value 红黑树
multimap<int, int> multimap1;


    multimap<int, float> multimap1;
    multimap1.emplace(make_pair(1, 1.0));
    multimap1.emplace(make_pair(2, 2.1));
    multimap1.emplace(make_pair(2, 2.2));
//// 访问mutimap key 的值（由于不能使用at和[]，只能进行范围访问）
//    auto itor = multimap1.find(2);  // 返回第一个key的value
//    cout << (*itor).first << (*itor).second << endl;

    // 1.lower_bound upper_bound
    auto lower_iter = multimap1.lower_bound(2);
    auto upper_iter = multimap1.upper_bound(2);
    for(auto i = lower_iter; i != upper_iter; ++i)
    {
        cout << i->first << i->second << endl;
    }


    // 2.equal_range
    auto equal_iter = multimap1.equal_range(2);
    auto lower_it1 = equal_iter.first;
    auto upper_it1 = equal_iter.second;
    for(auto i = lower_it1; i != upper_it1; ++i){
        cout << i->first << " " << i->second << endl;
    }

    // 3.find+count(map会自动排序）
    auto iter3 = multimap1.find(2);
    if(iter3 != multimap1.end())
    {
        for(size_t i = 0; i < multimap1.count(2); ++i)
        {
            cout << iter3->first << " " << iter3->second << endl;
            iter3++;
        }
    }


//unordered_map
//  map是一种有序的容器，底层是用红黑树实现的，红黑树是一种自平衡的二叉树，可以保障最坏情况的运行时间，它可以做到O(logn)时间完成查找、插入、删除元素的操作。       
//  unordered_map是一种无序的容器，底层是用哈希表实现的，哈希表最大的优点是把数据的查找和存储时间都大大降低。
unordered_map<int, int> unordermap1{{'1', 1}, { '2', 2 }, { '3', 3 }};
unordered_map<int, int> unordermap2(unordermap1.begin(), unordermap1.end());
unordermap2.insert(make_pair<int, int>('4', 4 ));
cout << unordermap2.bucket_count() << endl;

// set val不允许重复 红黑树
set<int> set1;
set<int> set2{1, 2, 3, 4, 5};
set<int> set3(set2);

// multiset 红黑树
multiset<int> muSet{1, 1, 1, 2, 3};
//unordered_set 
// set底层实现通常是红黑树，元素自动排序，但同时也造成了一个重要限制：不能直接改变元素值，因为这会打乱原本正确的顺序,查找函数具有对数复杂度.要改变元素的值，必须先删除该元素，再插入新元素
// unordered_set- 实现通常是hash-table元素是无序的插入、删除、查找元素的时间复杂度是常量的
unordered_set<int> uSet{1, 2, 2, 3, 5, 6};

```
# 多线程 线程池 
thread创建一定要join/detach，否则无法释放资源
```cpp
1.therad  // 不可复制 move 可以用=
// 创建完成后立刻运行传入的callback  主线程创建后一定要声明join或detach
join()
joinable()
detach()
this_thread::sleep_for()
this_thread::sleep_until()
this_thread::yield()
this_thread::get_id()
swap(thread1, thread2)


thread th1(fun)
thread th1(fun, x) // 线程有自己的栈空间，需要使用ref或指针
thread th1(fun, ref(x))
thread th1(fun, p) // int *p = &x;

// 类中非静态函数可加mem_fn  类成员函数转化为函数对象， 使用非boost::bind时需要加
therad th1(mem_fn(&AutoCalib::recheck_t), this) // 类中的引用
thread th1(&TT::tts, this); // 
// 类外的引用，实例化后
TheradFun theradFun;
thread th1(&ThreadFUN::Fun, &threadFun);  // 实例指针可不加

// promise future 异步访问
std::promise<int> p1;
std::future<int> fu1 = p1.get_future();
thread Thread1(fun, move(p1));
fun1.wait();
cout << fun1.get() << endl;


2.thread_pool 实现详细思路，目的，作用
boost::asio::thread_pool
shared_ptr<promise<bool>> mTheradStatus = nullptr;
boost::asio::thread_pool pool(5);

boost::asio::thread_pool pools(5);
boost::asio::post(pools, fun1); // 简单的函数指针可直接传入，最好都加
boost::asio::post(pools, &fun2);  // &可不加，涉及类的必须加 // 传指针时，不能直接&左值
boost::asio::post(pools, boost::bind(&fun4, ref(var)));

shared_ptr<std::promise<bool>> flag;
flag.reset(new std::promise<bool>);
boost::asio::post(pools, boost::bind(&fun5, flag)); // 建议使用boost::bind更通用

boost::asio::post(pools, boost::bind(& TT::tts, tt1));  // pool 必须用bind 传入类的callback function
pools.join(); // 可以不join，不阻塞运行

class threadClassTest {
public:
    int worker = 2;
    void printH() { cout << "H" << endl; }
    boost::asio::thread_pool mPool;
    threadClassTest() :mPool(2) {};
    void thread_t() { cout << "yes" << endl; }
    void test() {
    boost::asio::post(mPool, boost::bind(&threadClassTest::thread_t, this)); //类同实例化相同
    // 类中的boost::bind传递参数可以值传递、地址传递(不能直接&左值) 不可使用ref引用传递
	boost::asio::post(mPool, boost::bind(&A::fun, this, p)); 
}
};
```

![[Pasted image 20230927110859.png]]
线程池主要解决两个问题：
线程创建与销毁的开销以及线程竞争造成的性能瓶颈。
通过预先创建一组线程并复用它们，线程池有效地降低了线程创建和销毁的时间和资源消耗。同时，通过管理线程并发数量，线程池有助于减少线程之间的竞争，增加资源利用率，并提高程序运行的性能(过多的线程可能导致线程竞争，影响系统性能。线程池通过维护一个可控制的并发数量，有助于减轻线程之间的竞争)
1. 创建线程,(线程池在初始化时会预先创建一定数量的线程，这些线程将会被后续任务复用)
2. 任务队列与调度(线程池通过维护一个任务队列来管理待执行任务。当线程池收到一个新任务时，它会将任务加入到任务队列中。线程会按照预定策略（例如FIFO）从队列中取出任务执行。)
3. 线程执行及回收.(线程执行任务时，会遵循线程池的调度策略从任务队列中获取任务。任务完成后，线程将被放回到线程池中等待下一个任务，而不是销毁。这种复用机制提高了资源利用率并降低了线程创建销毁的开销。)
- 核心线程数(core_threads)：线程池中拥有的最少线程个数，初始化时就会创建好的线程，常驻于线程池。
- 最大线程个数(max_threads)：线程池中拥有的最大线程个数，max_threads>=core_threads，当任务的个数太多线程池执行不过来时，内部就会创建更多的线程用于执行更多的任务，内部线程数不会超过max_threads，多创建出来的线程在一段时间内没有执行任务则会自动被回收掉，最终线程个数保持在核心线程数

# 锁 条件变量 信号量

>> 同步 ：使用互斥量和条件变量 解决临界资源数据访问问题，线程同步化（阻塞等待）
>> 异步： 调用后不会等待返回结果，直接返回，线程完成后自动返回数据(通过条件变量通知状态更新或线程结束，通过future/promise异步访问数据)
```cpp
1. 互斥量 同步
std::mutex Mutex;
std::timed_mutex timedMutex;
std::recursive_mutex recursiveMutex;  // 可递归,解决自身多次获取锁引发的死锁问题
std::recursive_timed_mutex recursiveTimedMutex;
std::shared_mutex sharedMutex; // 搭配 shared_mutex


lock
unlock
lock_guard
unique_lock
shared_lock
// 相较于lock_guard 支持手动unlock
// 互斥锁mutex是为了临界资源的安全访问，C++11的unique_lock和lock_guard可以防止释放锁导致死锁问题
2. 锁
mutex mut1; 
shared_mutex smut1; 

shared_lock<std::shared_mutex> lock1(smut1); // 读锁
unique_lock<std::shared_mutex> lock2(smut1); // 写锁
lock_guard <std::shared_mutex> lock3(smut1); // 写锁
lock_guard<std::mutex> lock4(mut1); // 普通锁
// 超出作用域都自动解锁

```

```cpp
1. condition_variable  同步、异步（只能在线程）
// 必须用 unique_lock。原因是条件变量在wait时会进行unlock再进入休眠, lock_guard并无该操作接口
// 条件变量 std::condition_variable 是为了解决死锁而生，当互斥操作不够用而引入的。 比如，线程可能需要等待某个条件为真才能继续执行， 而一个忙等待循环中可能会导致所有其他线程都无法进入临界区使得条件为真时，就会发生死锁。 所以，condition_variable 实例被创建出现主要就是用于唤醒等待线程从而避免死锁。 
wait()
wait_for()
wait_until()
notify_one()
notify_all()

// 生产者消费者模式
mutex Mutex; // 保护wiat和判断条件
condition_variable conditionVariable;
deque<int> dataDeque;

const int MAX = 30;
const int PRODUCER_THREAD_NUM = 3;
const int CUSTOMER_THREAD_NUM = 5;
int index = 0;

void producer_thread(int threadid)
{
    while(true)
    {
        this_thread::sleep_for(std::chrono::milliseconds(500));  
        // 间隔500ms判断，减少判断消耗资源
        std::unique_lock<std::mutex> lock(Mutex);
        // condition_variable 一定搭配mutex使用，应为判断条件是临界资源(保证全局条件和wait是原则操作)
        if(dataDeque.size() <= MAX)
        {
            index++;
            dataDeque.push_back(index);  // 任务为全局变量
            cout << "produce thread id" << threadid << " " << index << endl;
            cout << "dataDeque size" << dataDeque.size() << endl;
        }
        else
        {
            conditionVariable.notify_all();
            conditionVariable.notify_one(); // 随机唤醒一个休眠的线程
        }
    }
}

void customer_thread(int threadid){
    while(true){
        this_thread::sleep_for(std::chrono::milliseconds(500));
        std::unique_lock<std::mutex> lock(Mutex);
        if(!dataDeque.empty()){
            int data = dataDeque.front();
            dataDeque.pop_front();
            cout << "customer thread id" << threadid << endl;
            cout << "data:" << data << "dataDeque size" << dataDeque.size() << endl;
        }
        else{
            // wait(lock)
            conditionVariable.wait(lock);
            

            // wait(lock, condition)
            // 增加第二个参数，判断条件是否唤醒，解决虚假虚假唤醒(notify_all 通知了但没有产品)
            conditionVariable.wait(lock, []()->bool{return !dataDeque.empty();});

            // wait_for() 阻塞时长超过后返回
            conditionVariable.wait_for(lock, std::chrono::milliseconds(1000));
            
            // wait_until() 阻塞超过指定时间点
            conditionVariable.wait_until(lock, std::chrono::steady_clock::now() + std::chrono::milliseconds(100));


        }
    }
}

int main(){
    thread *proThread[PRODUCER_THREAD_NUM];
    thread *cusThread[CUSTOMER_THREAD_NUM];

    for(int i = 0; i < PRODUCER_THREAD_NUM; ++i){
        proThread[i] = new thread(producer_thread, ref(i));
    }

    for(int j = 0; j < CUSTOMER_THREAD_NUM; ++j){
        cusThread[j] = new thread(customer_thread, ref(j));
    }
    // join()
    for(int i = 0; i < PRODUCER_THREAD_NUM; ++i){
        proThread[i]->join();
    }
    for(int j = 0; j < CUSTOMER_THREAD_NUM; ++j){
        cusThread[j]->join();
    }

}
```
实现多个线程之间的同步操作，当条件不满足时，相关线程一直被阻塞，直到某种条件成立，这些线程才会被唤醒。
条件变量是利用线程间共享的''全局变量''进行同步的一种机制，主要包含两个动作：
一个线程因为等待条件变量的条件成立而挂起，另外一个线程使条件成立，给出信号，从而唤醒被等待的线程。
***为了防止竞争，条件变量总是和一个互斥锁结合在一起，通常情况下这个锁是 std::mutex，并且管理这个锁的只能是std::unique_lock<std::mutex> RAII 的模板类。***

临界资源：
临界资源是一次执行过程仅仅允许一个进程使用的共享资源，各个进程采取互斥的方式实现共享，属于临界资源的硬件有 打印机，磁带机等，软件有消息队列，变量，数组，缓冲区等，各个进程采取互斥的方式实现对这种资源的共享。（可以理解为对资源的一次操作不能被打断，也就是一次操作过程需要完整，不能操作出现中间切走的情况）

临界区：
每个进程访问临界资源的那段代码称为临界区，每次只允许一个进程进去临界区，进去后，不允许其他进程进入。不论是硬件临界资源还是软件临界资源，多个进程必须互斥的对它进行访问，使用临界区时，一般不允许其运行时间过长，只要运行在临界区的线程还没有离开，其他所有进入此临界区的线程都会被挂起而进入等待状态，并在一定程度上影响程序的运行性能。

原子操作：
原子操作是指不会被线程调度机制打断的操作；

```cpp 
// 无名信号量
sem_t  sem;// 同步（进程、线程）
sem_init(&sem, 0, 0) 0：当前进程局部信号量 否则其他进程可访问 0：初始化值
sem_destory(&sem);
sem_wait(&sem) 原子操作 -- sem_wait() 递减(锁定)由 sem 指向的信号量。如果信号量的值大于零，那么递减被执行，并且函数立即返回。如果信号量的当前值是零，那么调用将阻塞到它可以执行递减操作为止(如信号量的值又增长超过零)，或者调用被信号打断

struct timespec ts;
clock_gettime(CLOCK_MONOTONIC, &ts);
ts.tv_sec += 5;
sem_timedwait(&sem, &ts) -- sem_timedwait() 与 sem_wait() 类似，只不过 _abs_timeout_ 指定一个阻塞的时间上限，如果调用因不能立即执行递减而要阻塞。


sem_trywait 原子操作 不wait -- 如果递减不能立即执行，调用将返回错误(_errno_ 设置为EAGAIN)而不是阻塞。
sem_post  原子操作 v++

int semVal;
sem_getvalue(&sem, &semVal);

// 有名信号量 一个文件
sem_open(const char *name) // 打开
sem_close() // 关闭
sem_unlink() // 删除

------------------------------------------
// sem_t counting_semaphore 非负计数 binary_semaphore 二值计数 C++20
sem_t sem;
mutex Mutex;
condition_variable cvar;
void fun1(void *param){
    cout << __func__ << ": wait..." << endl;
    sem_wait(&sem);
    struct timespec ts;
    clock_gettime(CLOCK_MONOTONIC, &ts);
    ts.tv_sec += 5;
    sem_timedwait(&sem, &ts);
//    unique_lock<std::mutex> lock(Mutex);
//    cvar.wait(lock);
    int *running = (int*) param;
    cout << "fun1 running:" << *running << endl;
}

void fun2(int time){
    cout << __func__ << " " << time << endl;
    sem_post(&sem);
    int semVal;
    sem_getvalue(&sem, &semVal);
    cout << "semVal:" << semVal << endl;
//    cvar.notify_one();  // 1.条件变量不能通知指定线程 2.更麻烦需要现lock
}
int main(){
    sem_init(&sem, 0, 0);
    int time = 5;
    thread th1(fun1, &time);
    thread th2(fun2, time);
    if(th1.joinable()) th1.join();
    if(th2.joinable()) th2.join();
    sem_destroy(&sem);
    return 0;
}
```
> 1.用法上：信号量更像是条件变量和互斥锁的组合,使用会比条件变量的使用更加灵活
> 2.场景上：信号量即可用于进程间同步，也可用于线程间同步;条件变量只能用于线程间
> 3.底层实现：信号量内部以计数方式来判定是否阻塞（有累计数量的功能），计数单次加减1即每次只能唤醒一个进程或线程。而条件变量以“自定义条件”来判定是否阻塞（优点来了），那么它就可以唤醒一个线程或所有线程（notify_all类似广播）。

# 线程安全
**主线程结束，子线程也会被迫结束，除非使用detach**
一个类或者程序所提供的接口对于线程来说是原子操作或者多个线程之间的切换不会导致该接口的执行结果存在二义性,也就是说我们不用考虑同步的问题。
若有多个线程同时执行写操作，一般都需要考虑线程同步，否则的话就可能影响线程安全。

# 死锁
1. 互斥条件：指进程对所分配到的资源进行排它性使用，即在一段时间内某资源只由一个进程占用。如果此时还有其它进程请求资源，则请求者只能等待，直至占有资源的进程用毕释放
2. 请求和保持条件：指进程已经保持至少一个资源，但又提出了新的资源请求，而该资源已被其它进程占有，此时请求进程阻塞，但又对自己已获得的其它资源保持不放
3. 不剥夺条件：指进程已获得的资源，在未使用完之前，不能被剥夺，只能在使用完时由自己释放。
4. 环路等待条件：在发生死锁时，必然存在一个进程——资源的环形链，即进程集合{P0，P1，P2，···，Pn}中的P0正在等待一个P1占用的资源；P1正在等待P2占用的资源，……，Pn正在等待已被P0占用的资源。
破解死锁：
1. 破坏互斥：线程运行初始时，分别拷贝一份资源（拷贝成本高）
2. 破坏请求保持：资源释放后才能获取下一个资源
3. 破坏不可剥削：超时自身释放资源，添加线程优先级
4. 破坏循环等待：强制规定资源获取顺序
```cpp
int A = 10;
int B = 20;
mutex Mutex1;
mutex Mutex2;
void fun1(){
    // 1 破坏互斥
//    int A1 = A;
//    int B1 = B;
    unique_lock<std::mutex> lock1(Mutex1);
    ++A;
    cout << "get A" << A << endl;
    // 2 破坏请求保持 
//    lock1.unlock();  // 或者使用atomic<int> A;
    // 3 破坏不可剥削
//    int t    ;
//    while(t > -1){
//        t--;
//        if(t < 0){
//            return;
//        }
//    }
    unique_lock<std::mutex> lock2(Mutex2);
    ++B;
    cout << "get B" << B << endl;
}
void fun2(){
    // 4 破坏循环等待 强制Mutex1 Mutex2
    unique_lock<std::mutex> lock1(Mutex2);
    ++B;
    cout << "get B" << B << endl;
    // lock1.unlock();
    unique_lock<std::mutex> lock2(Mutex1);
    ++A;
    cout << "get A" << A << endl;
}
int main(){
    thread th1(fun1);
    thread th2(fun2);
    th1.join();
    th2.join();
    return 0;
}
```

# 高级特性
1. atomic 原子类型 ---不支持浮点数---
原子类型避免了锁的使用, 能大大的提高程序的运行效率。
对于一些变量不希望有中间状态，不会发生读写问题，使用原子类型而不是mutex会方便很多 
sem_t就是原子操作

atomic使用无锁编程（**lock free**——不使用锁来保持代码同步）。

原子操作是一种以单个事务来执行的操作，这是一个“不可再分且不可并行的”操作，其他线程只能看到操作完成前或者完成后的资源状态，不存在中间状态可视

使用原子类型一定是线程安全的吗？
不是的。
nonatomic的内存管理语义是非原子性的，非原子性的操作本来就是线程不安全的，而atomic的操作是原子性的，但是并不意味着它是线程安全的，它会增加正确的几率，能够更好的避免线程的错误，但是它仍然是线程不安全的。
当使用nonatomic的时候，属性的setter，getter操作是非原子性的，所以当多个线程同时对某一属性读和写操作时，属性的最终结果是不能预测的。
当使用atomic时，虽然对属性的读和写是原子性的，但是仍然可能出现线程错误：当线程A进行写操作，这时其他线程的读或者写操作会因为该操作而等待。当A线程的写操作结束后，B线程进行写操作，然后当A线程需要读操作时，却获得了在B线程中的值，这就破坏了线程安全，如果有线程C在A线程读操作前release了该属性，那么还会导致程序崩溃。所以仅仅使用atomic并不会使得线程安全，我们还要为线程添加lock来确保线程的安全。
也就是要注意：atomic所说的线程安全***只是保证了getter和setter存取方法的线程安全，并不能保证整个对象是线程安全的***

```cpp
atomic_int atomic<int>
atomic_bool atomic<bool>
atomic_char atomic<char>
atomic_uint atomic<uint>
atomic_long atomic<long>


is_lock_free() // 是否支持无锁操作
atomic_flag flag;  // 原子布尔类型，实现原子锁操作
flag.test_and_set(std::memory_order_acquire) // 测试并设置原子锁标志位
flag.clear(std::memory_order_acquire) // 清楚标志位，模拟原子操作

atomic<int> a;
// atomic<int> a = 1; // 错！！！
atomic<int> a(1); // 对
a.store(0);
a.load();
int b = a.exchange(c) // b获取a，a赋值为c
atomic_compare_exchange_strong(&a, &b, &c) // if a==b 则a=c 保证原子性
atomic_compare_exchange_weak  // 不保证原子性
a.fetch_add(10) // a+=10  类似于i++ 值在后续发生改变
a.fetch_sub(10) // a-=10
a.fetch_and() // 与
a.fetch_or() // 或
a.fetch_xor() // 异或

// 是否会出现数据等待 不是死锁
    struct sa{long x; int y;}; // no lock free
    atomic<sa> aa{};
    cout << aa.is_lock_free() << endl;
```
2. promise future 提供异步访问
	promise和future不可以拷贝，因此传递需要用&或初始化为指针（shared_ptr)，或者使用move，但注意move之后将失效，不能再次使用(获取)，不建议
```cpp
    std::promise<int> pp1;
//    std::promise<int> pp2(pp1);  // 除了move，promise 和 future 不允许拷贝
//    std::promise<int> pp3 = pp1;
    std::promise<int> pp4 = std::move(pp1);
    std::future<int> ff1 = pp4.get_future();
//    std::future<int> ff2 = ff1;
//    std::future<int> ff3(ff1);
    std::future<int> ff4 = std::move(ff1);


-----------------------------------------------------
void fun(std::promise<int> &p1){  // &
    p1.set_value(20);
    cout << __func__ << "is end" << endl;
}
void fun2(std::future<int> f2){ // 直接传递 使用move
    int x = f2.get();
    cout << "f2:" << x << endl;
}
void fun3(std::shared_ptr<std::promise<int>> promisePtr){  // 使用shared_ptr
    promisePtr->set_value(120);
}


    std::promise<int> p1;
    std::future<int> f1 = p1.get_future();
    thread th1(fun, ref(p1)); // 父线程获取子线程的值
//    f1.wait();
    cout << "f1:" << f1.get() << endl;
    th1.join();  // 创建线程后要告知join/detach!!!

    // 由于promise及future不可复制，需要通过&传递或初始化为智能指针再直接传递
    std::shared_ptr<std::promise<int>> promisePtr;
    promisePtr.reset(new std::promise<int>); // 这里是shared_ptr的方法reset();
    thread th3(fun3, promisePtr);
    cout << "fun3:" << promisePtr->get_future().get() << endl;
    th3.join();


    std::promise<int> p2;
    std::future<int> f2 = p2.get_future(); // 关联
    thread th2(fun2, std::move(f2));  // 子线程获取父线程的值
    p2.set_value(20);
    th2.join();
    
```

3. 初始化方式{}和列表初始化
注意：STL中stack不支持{}初始化，依赖于其他容器(deque)拷贝构造
```cpp
int x1{10};
int arrx1[]{1, 2, 3};
vector<int> vec{1, 2, 3};
map<int, float> map1{{1, 1.0f}, {2, 2.0f}};
class B{
    public:
    B(int x, int y):X(x), Y(y){}
    int X;
    int Y;
};
```

4. auto decltype
auto 运行期间才能确定类型
decltype 编译期间根据表达式确定
```cpp
auto it = vector1.begin() // vector<int>::iterator
decltype(vector1.begin()) it;
decltype(a + b) it2;
```

5. 类的修改符 override final default delete 
override: 仅修饰派生类继承基类的同名虚函数 强制要求重载
final：修饰类表示不可继承 修饰虚函数表示不可修改（子类不可再操作）
default： 默认成员函数保留
delete： 默认成员函数删除

```cpp
    class A final{};  // 修饰类，不可继承
//    class B : public A {};
    class A1{virtual void fun() final{cout << "1" << endl;}}; // 修饰虚函数(只能) 不可修改
//    class B1:A1{virtual void fun()};

    class A2{virtual void  fun() {}};
    class B2:A2{virtual void fun() override{cout << "1";}}; // 检查派生类虚函数必须重载函数

    // 4 default delete 默认成员函数控制
    class A3{
    public:
        A3() = default;
        A3(const A&) = delete;
        A3& operator=(const A&) = delete;
        A3(int a):_a(a){};
        int _a;
    };
```
6. 右值引用&&  完美转发 forward  移动语义 move
```cpp
1.左值：可取地址&可修改（除了const）的值
int *p = new int(10);
int al = 10;
const int bl = 20;
左值引用：（常亮指针）
int * &pll = p;
int & all = al;
const int & bll = bl;
右值：常亮、函数返回值、表达式返回值（将亡值，比如函数返回对象接受就会销毁），不可修改
10；
add(1+2);
x+y;
// 10 = 1; no
// add(1+2) = 20; no
// x + y = 5; no
右值引用：
int&& rr1 = 10;
int&& rr2 = x + y;
int&& rr3 = add(1 + 2);
int&& rr4 = move(al); // move转移
使用右值引用调用移动构造和移动复制，减少拷贝消耗
    list<string> lt;
    string s("123");
    lt.push_back(s); // 左值
    lt.emplace_back("123"); // 右值, 建议emplace_back
    lt.emplace_back("123"); // 右值
    lt.push_back(std::move(s)); // 右值

2.完美转发 PerfectForward()根据输入值自动选择拷贝构造还是移动语义（仅仅改变类型）
template<typename T>
void PerfectForward(T&& t){ // && 可以引用左值或右值
    F1(forward<T>(t));
}
    PerfectForward(1); // 右
    int aP = 10;
    PerfectForward(aP); // 左
    PerfectForward(move(aP));  // 右
    
3.move 将左值引用转为右值引用 获取资源 避免内存拷贝 (仅仅改变类型)
```

7. lambda

```cpp
    vector<float> ff = {20.0, 31.0, 1.0, 15.0};
    sort(ff.begin(), ff.end(), [](float a, float b)->bool{
        return a > b;
    });
    for(auto &i:ff){cout << i;}
    int as = 10, bs = 20;
    auto lambdaFun = [=, &bs](int c)->int{return bs = as + c;};
    cout << "lambdaFun" << lambdaFun(100) << endl;
```

8. 可变参数列表
```cpp
template<class ...Args>
void showList(Args... args){
    cout<< sizeof...(args) << endl;
}
    showList(1, 2, 3);  // 3
    showList(1, 'A', string("sort"));  // 3
```
9. 智能指针 shared_ptr unique_ptr weak_ptr
```cpp
1.shared_ptr
 int ashare = 10;
shared_ptr<int> ptra = std::make_shared<int>(a); // 初始化不可用= 可以（）
shared_ptr<int> ptra2(ptra); // copy
int bshare = 20;
shared_ptr<int> ptrb = std::make_shared<int>(bshare);
shared_ptr<int> ptrc(new int 20)

cout << *ptra2;
ptra.reset(new int (10));
ptra.release();
ptra.get();
ptra.uniuqe();
ptra.use_count();
swap(ptra, ptrb);

int *pb = &a;
ptra2 = ptrb; // 重新指向
pb = ptrb.get(); // 原始指针
cout << ptra.use_count() << endl;
cout << ptrb.use_count() << endl;
cout << ptra.unique() << ptrb.unique() << endl;
cout << ptra.get() << ptrb.get() << endl;
swap(ptra, ptrb);
cout << ptra.get() << ptrb.get() << endl;
ptra.reset();
cout << ptra.get() << ptra.use_count() << endl;

2.unique_ptr 

std::unique_ptr<int> uptr(new int (10)); // 动态对象
int uniqueA = 20;
std::unique_ptr<int> uptr2 = make_unique<int>(uniqueA);

不可拷贝或复制
//    unique_ptr<int> uptr3 = uptr2;
//    unique_ptr<int> uptr4(uptr2);  

 
通过move可以转移所有权
std::unique_ptr<int> uptr5 = std::move(uptr2); 
uptr2.release();

3.weak_ptr
//weak_ptr 从 shared_ptr 构造 一般伴随shared_ptr使用 或直接构造 .lock转shared_ptr
    shared_ptr<int> sh_ptr = std::make_shared<int>(10);
    cout << sh_ptr.use_count() << endl;
    weak_ptr<int> wp(sh_ptr);  
    // weak_ptr 从 shared_ptr 构造 一般伴随shared_ptr使用
    cout << wp.use_count() << " " << sh_ptr.use_count() << endl;
// .expired() == (.user_count = =0)
    if(!wp.expired()){
        shared_ptr<int> sh_ptr2 = wp.lock(); // weak_ptr --> shared_ptr
        *sh_ptr = 100;
        cout << wp.use_count() << " " << sh_ptr.use_count() << endl;
    }
    // 循环引用
    class BB;
    class AA{
    public:
        AA(){cout << "AA() called" << endl;}
        ~AA(){cout << "~AA() called" << endl;}
        weak_ptr<BB> m_bb_ptr;
    };
    class BB{
    public:
        BB(){cout << "BB() called" << endl;}
        ~BB(){cout << "~BB() called" << endl;}
        shared_ptr<AA> m_aa_ptr;
    };

    shared_ptr<AA> ptr_a(new AA);
    shared_ptr<BB> ptr_b(new BB);
    {
        cout << ptr_a.use_count() << " " << ptr_b.use_count() << endl;
        cout << ptr_a->m_bb_ptr.use_count() << " " << ptr_b->m_aa_ptr.use_count() << endl;
        ptr_a->m_bb_ptr = ptr_b;
        cout << ptr_a.use_count() << " " << ptr_b.use_count() << ptr_a->m_bb_ptr.use_count() << " "
             << ptr_b->m_aa_ptr.use_count() << endl;
        ptr_b->m_aa_ptr = ptr_a;
        cout << ptr_a.use_count() << " " << ptr_b.use_count() << ptr_a->m_bb_ptr.use_count() << " "
             << ptr_b->m_aa_ptr.use_count() << endl;
    }
    cout << ptr_a.use_count() << ":" << ptr_b.use_count() << endl;
```

# 数据结构
```cpp
1. List
2. Stack
3. Deque
4. String
5. Tree
6. Map
```

# 设计原则
1. 单一职责
单一职责原则指出**一个类应该做一件事，因此它应该只有一个改变的理由**。软件规范中只有一个潜在的更改（数据库逻辑、日志记录逻辑等）应该能够影响类的规范。
2. 开闭原则
实体应该对**扩展开放，对修改关闭**。修改意味着更改现有类的代码，扩展意味着添加新功能。
我们应该能够在不触及类现有代码的情况下添加新功能。这是因为每当我们修改现有代码时，我们都在冒着产生潜在错误的风险。因此，如果可能，我们应该避免接触经过测试且可靠（大部分）的生产代码。

我们如何在不触及类的情况下添加新功能。它通常是**在接口和抽象类的帮助下完成的**。
3. 里氏替换
继承与派生的规则，子类能够完全替代父类（父类不要出现子类中不能继承的属性）
4. 接口隔离
隔离意味着将事物分开，而接口隔离原则是关于分离接口。许多特定于客户端的接口优于一个通用接口，不应强迫客户实现他们不需要的功能。
一个类对另外一个类的依赖应该建立在最小的接口之上。要为各个类建立他们需要的专用接口，而不要试图建立一个庞大的接口供所有依赖它的类去调用。程序员应该尽量将臃肿庞大的接口拆分成更小的和更具体的接口，让接口中只包含客户感兴趣的方法
5. 依赖倒转
**依赖关系应该是抽象的而不是具体的**。实现高层业务和实现层之间的解耦合；（业务层不应该考虑api接口）
- 高层模块不应该依赖于低层模块，二者都应该依赖于抽象；
- 抽象不应该依赖于细节，细节应该依赖于抽象。
6. 迪米特法则（最少知识原则）
降低类之间的耦合。
低耦合，高内聚.一个类对于其他类知道的越少越好，就是说一个对象应当对其他对象有尽可能少的了解,只和朋友通信，不和陌生人说话
7. 组合/聚合复用原则
尽量使用组合和聚合少使用继承的关系来达到复用的原则
Heart 和 Student、Teacher 都是一种 “强” 关联，人不能摆脱心脏而存在，即组合关系，而 Student 和 Class、School 是一种 “弱” 关联，脱离了学校、班级，学生还能属于社会或其他团体，即聚合关系。

# 设计模式
单例模式
简单工厂模式 工厂+抽象产品（具体产品）
工厂模式 n个工厂+n个产品
抽象工厂模式 抽象产品类+具体产品类 抽象工厂类+具体工厂类
```cpp
    class singleInstance
    {
        public:
        static singleInstance& GetInstance()
        {
            static singleInstance instance;
            return instance;
        }

        private:
        singleInstance() = default;
        ~singleInstance() = default;
        singleInstance(const singleInstance&) = delete;
        singleInstance& operator=(const singleInstance&) = delete;
    };

```

```cpp
// 鞋子抽象类
class Shoes
{
public:
    virtual ~Shoes() {}
    virtual void Show() = 0;
};

// 耐克鞋子
class NiKeShoes : public Shoes
{
public:
    void Show()
    {
        std::cout << "我是耐克球鞋，我的广告语：Just do it" << std::endl;
    }
};

// 阿迪达斯鞋子
class AdidasShoes : public Shoes
{
public:
    void Show()
    {
        std::cout << "我是阿迪达斯球鞋，我的广告语:Impossible is nothing" << std::endl;
    }
};


enum SHOES_TYPE
{
    NIKE,
    ADIDAS
};
1个工厂 n个产品
// 总鞋厂
class ShoesFactory
{
public:
    // 根据鞋子类型创建对应的鞋子对象
    Shoes *CreateShoes(SHOES_TYPE type)
    {
        switch (type)
        {
        case NIKE:
            return new NiKeShoes();
            break;
        case ADIDAS:
            return new AdidasShoes();
            break;
        default:
            return NULL;
            break;
        }
    }
};
```

```cpp
n 个工厂
// 总鞋厂
class ShoesFactory
{
public:
    virtual Shoes *CreateShoes() = 0;
    virtual ~ShoesFactory() {}
};

// 耐克生产者/生产链
class NiKeProducer : public ShoesFactory
{
public:
    Shoes *CreateShoes()
    {
        return new NiKeShoes();
    }
};

// 阿迪达斯生产者/生产链
class AdidasProducer : public ShoesFactory
{
public:
    Shoes *CreateShoes()
    {
        return new AdidasShoes();
    }
};

```


抽象工厂
```cpp
// 基类 衣服
class Clothe
{
public:
    virtual void Show() = 0;
    virtual ~Clothe() {}
};

// 基类 鞋子
class Shoes
{
public:
    virtual void Show() = 0;
    virtual ~Shoes() {}
};

// 耐克衣服
class NiKeClothe : public Clothe
{
public:
    void Show()
    {
        std::cout << "我是耐克衣服，时尚我最在行！" << std::endl;
    }
};

// 耐克鞋子
class NiKeShoes : public Shoes
{
public:
    void Show()
    {
        std::cout << "我是耐克球鞋，让你酷起来！" << std::endl;
    }
};

// 总厂
class Factory
{
public:
    virtual Shoes *CreateShoes() = 0;
	virtual Clothe *CreateClothe() = 0;
    virtual ~Factory() {}
};

// 耐克生产者/生产链
class NiKeProducer : public Factory
{
public:
    Shoes *CreateShoes()
    {
        return new NiKeShoes();
    }
	
	Clothe *CreateClothe()
    {
        return new NiKeClothe();
    }
};


```

```cpp
int main()
{
    // ================ 生产耐克流程 ==================== //
    // 鞋厂开设耐克生产线
    Factory *niKeProducer = new NiKeProducer();
    
	// 耐克生产线产出球鞋
    Shoes *nikeShoes = niKeProducer->CreateShoes();
	// 耐克生产线产出衣服
    Clothe *nikeClothe = niKeProducer->CreateClothe();
    
	// 耐克球鞋广告喊起
    nikeShoes->Show();
	// 耐克衣服广告喊起
    nikeClothe->Show();
	
    // 释放资源
    delete nikeShoes;
	delete nikeClothe;
    delete niKeProducer;


    return 0;
}


```