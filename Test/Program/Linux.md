
多进程的通信方式
1. 管道通信
```cpp
 int fd[2];
    pid_t pid;
    char buffer[256];
    // create pipe
    if(pipe(fd) < 0){
        cerr << "Failed to create pipe." << endl;
        return 1;
    }
    // create child progress
    if((pid = fork()) < 0) {
        cerr << "failed" << endl;
        return 1;
    }
    else if(pid > 0){
        close(fd[0]);
        string message = "child progress";
        write(fd[1], message.c_str(), message.length());
        wait(NULL);
    }else {
        close(fd[1]);
        read(fd[0], buffer, sizeof(buffer));
        cout << "message from parent process" << buffer << endl;
    }
```
2. 共享内存
```cpp
#include <iostream>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <unistd.h>

int main() {
    // 创建共享内存
    int shmid = shmget(IPC_PRIVATE, sizeof(int), IPC_CREAT | 0666);
    if (shmid == -1) {
        std::cerr << "Failed to create shared memory" << std::endl;
        return 1;
    }

    // 连接共享内存
    int* sharedData = (int*)shmat(shmid, nullptr, 0);
    if (sharedData == (int*)-1) {
        std::cerr << "Failed to attach shared memory" << std::endl;
        return 1;
    }

    // 写入数据
    *sharedData = 42;

    // 创建子进程
    pid_t pid = fork();
    if (pid == -1) {
        std::cerr << "Failed to create child process" << std::endl;
        return 1;
    }

    if (pid == 0) {
        // 子进程读取共享内存中的数据
        std::cout << "Child process: " << *sharedData << std::endl;

        // 分离共享内存
        if (shmdt(sharedData) == -1) {
            std::cerr << "Failed to detach shared memory in child process" << std::endl;
            return 1;
        }
    } else {
        // 父进程等待子进程结束
        wait(nullptr);

        // 分离共享内存
        if (shmdt(sharedData) == -1) {
            std::cerr << "Failed to detach shared memory in parent process" << std::endl;
            return 1;
        }

        // 删除共享内存
        if (shmctl(shmid, IPC_RMID, nullptr) == -1) {
            std::cerr << "Failed to delete shared memory" << std::endl;
            return 1;
        }
    }

    return 0;
}
```
4. 消息传递（消息队列）
```cpp
#include <iostream>
#include <sys/ipc.h>
#include <sys/msg.h>
#include <unistd.h>

struct Message {
    long type;
    int data;
};

int main() {
    // 创建消息队列
    int msgid = msgget(IPC_PRIVATE, IPC_CREAT | 0666);
    if (msgid == -1) {
        std::cerr << "Failed to create message queue" << std::endl;
        return 1;
    }

    // 创建子进程
    pid_t pid = fork();
    if (pid == -1) {
        std::cerr << "Failed to create child process" << std::endl;
        return 1;
    }

    if (pid == 0) {
        // 子进程发送消息
        Message message;
        message.type = 1;
        message.data = 42;

        if (msgsnd(msgid, &message, sizeof(message.data), 0) == -1) {
            std::cerr << "Failed to send message in child process" << std::endl;
            return 1;
        }
    } else {
        // 父进程接收消息
        Message message;

        if (msgrcv(msgid, &message, sizeof(message.data), 1, 0) == -1) {
            std::cerr << "Failed to receive message in parent process" << std::endl;
            return 1;
        }

        std::cout << "Parent process: " << message.data << std::endl;

        // 删除消息队列
        if (msgctl(msgid, IPC_RMID, nullptr) == -1) {
            std::cerr << "Failed to delete message queue" << std::endl;
            return 1;
        }
    }

    return 0;
}

```