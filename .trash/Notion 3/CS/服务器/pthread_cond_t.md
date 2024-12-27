- 可以类比成xv6中的sleep和wakeup
- 条件变量用于线程之间的同步，可以使一个或多个线程等待某个特定的条件发生。
- 条件变量与互斥锁（mutex）配合使用

# 使用步骤

- 初始化条件变量：
    - 静态初始化：`pthread_cond_t m_cond = PTHREAD_COND_INITIALIZER;` (编译时）
    - 动态初始化：`pthread_cond_init(&m_cond, NULL);` （运行时）
- 等待条件变量：
    - 当线程需要等待某个条件时，它可以调用 `pthread_cond_wait(&m_cond, &mutex);`。这个函数会**释放互斥锁**并使线程进入等待状态，直到条件变量被通知。
    - `pthread_cond_wait` 函数的返回值是一个整数，用于指示函数是否成功执行。如果函数成功执行并返回 0，则表示线程成功地等待到条件变量。如果函数返回非零值，则表示出现了错误。
    - `pthread_cond_timedwait`允许线程在等待条件变量的同时指定一个超时时间。如果在指定的时间内没有被唤醒，线程将自动超时并返回。这个函数可以有效地防止线程无限期地等待某个条件发生，从而避免可能的死锁情况。
- 通知等待的线程：
    - `pthread_cond_signal(&m_cond);`：唤醒一个等待在该条件变量上的线程。
    - `pthread_cond_broadcast(&m_cond);`：唤醒所有等待在该条件变量上的线程。
- 销毁条件变量：  
    当条件变量不再需要时，可以使用  
    `pthread_cond_destroy(&m_cond);` 来销毁它。

# 示例

- 以下例子确保子线程一定在主线程 “ready” 之后才会执行

```C
\#include <stdio.h>
\#include <pthread.h>

pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t cond = PTHREAD_COND_INITIALIZER;
int ready = 0;

void* thread_func(void* arg) {
    pthread_mutex_lock(&mutex);
    while (!ready) {
        pthread_cond_wait(&cond, &mutex);
    }
    printf("Thread %d: Condition met, proceeding.\n", *(int*)arg);
    pthread_mutex_unlock(&mutex);
    return NULL;
}

int main() {
    pthread_t threads[2];
    int thread_ids[2] = {1, 2};

    // 创建线程
    for (int i = 0; i < 2; i++) {
        pthread_create(&threads[i], NULL, thread_func, &thread_ids[i]);
    }

    // 模拟条件满足
    sleep(1); // 模拟一些工作
    pthread_mutex_lock(&mutex);
    ready = 1;
    pthread_cond_broadcast(&cond); // 通知所有等待线程
    pthread_mutex_unlock(&mutex);

    // 等待线程完成
    for (int i = 0; i < 2; i++) {
        pthread_join(threads[i], NULL);
    }

    // 销毁条件变量和互斥锁
    pthread_cond_destroy(&cond);
    pthread_mutex_destroy(&mutex);

    return 0;
}
```