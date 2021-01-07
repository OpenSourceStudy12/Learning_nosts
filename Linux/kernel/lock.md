# 1. 原子操作
```
define:
typedef struct { volatile int counter; } atomic_t;

read:
atomic_read(atomic_t *v):
#define atomic_read(v)	(*(volatile int *)&(v)->counter)

write:
atomic_set(atomic_t *v, int i):
#define atomic_set(v,i)	(((v)->counter) = (i))

add:
void atomic_add(int i, atomic_t *v);
void atomic_inc(atomic_t *v);// 计数值加1
int atomic_add_return(int i, atomic_t *v);//加i后 返回atomic 计数值
int atomic_add_negative(int i, atomic_t *v);// 加i后 计数值小于0返回1

sub:
void atomic_sub(int i, atomic_t *v);
void atomic_dec(atomic_t *v); // 计数值减1
int atomic_sub_return(int i, atomic_t *v);//减i后 返回atomic 计数值
int atomic_sub_test(int i, atomic_t *v);//减i后计数值为0返回1
```
# 2. 信号量
Linux内核的信号量在概念和原理上与用户态的System V的IPC机制信号量是一样的，但是它绝不可能在内核之外使用，因此它与System V的IPC机制信号量毫不相干。
信号量在创建时需要设置一个初始值，表示同时可以有几个任务可以访问该信号量保护的共享资源，初始值为1就变成互斥锁（Mutex），即同时只能有一个任务可以访问信号量保护的共享资源。一个任务要想访问共享资源，首先必须得到信号量，获取信号量的操作将把信号量的值减1，若当前信号量的值为负数，表明无法获得信号量，该任务必须挂起在该信号量的等待队列等待该信号量可用；若当前信号量的值为非负数，表示可以获得信号量，因而可以立刻访问被该信号量保护的共享资源。当任务访问完被信号量保护的共享资源后，必须释放信号量，释放信号量通过把信号量的值加1实现，如果信号量的值为非正数，表明有任务等待当前信号量，因此它也唤醒所有等待该信号量的任务。
```

```

# 3. 自旋锁 spinlock
- spin_lock
- spin_lock_irq
- spin_lock_irqsave
# 4. 互斥锁 mutex
# 5. 读写锁 rwlock