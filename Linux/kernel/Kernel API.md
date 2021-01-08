# The Linux Kernel API

## 1. List Management Functions

```
1.1 void INIT_LIST_HEAD(struct list_head *list)

Desc: 初始化 list 指向自己

1.2 #define LIST_HEAD_INIT(name) {&(name), &{name}}

Desc: 初始化name 指向自己

2 #define LIST_HEAD(name)

Desc: 定义并初始化名为name的 list_head 变量

3.1 void list_add(struct list_head *new, struct list_head *head)
Desc: 添加new 到 head 后面

3.2 void list_add_tail(struct list_head *new, struct list_head *head)
Desc: 添加new 到 head 前面

```

list_add 和 list_add_tail

![list_head](../../resource/list_head.png)

```
4.1 void list_del(struct list_head *entry)
Desc: 从 list 删除 entry

4.2 void list_del_init(struct list_head *entry)
Desc: 从 list 删除 entry, 病初始化 entry

5.1 void list_replace(strut list_head *old, struct list_head *new)
Desc：用 new 替换 list 中的 old

5.2 void list_replace_init(struct list_head *old, struct list_head *new)
Desc: 用 new 替换 list 中的 old, 并初始化old

6.1 voud list_move(struct list_head *list, struct list_head *head)
Desc: 从一个链表删除，后 insert to after head

6.2 void list_move_tail(struct list_head *list, struct list_head *head)
Desc: 从一个链表删除， 后 insert to before head 

7 int list_is_last(const struct list_head *list, const struct list_head *head)
Desc: check list 是否是 head 中最后一个节点

8.1 int list_empty(struct list_head *head)
Desc: check head 链表是否为空

8.2 int list_empty_careful(struct list_head *head)
Desc: check head 链表是否为空

9 void list_rotate_left(struct list_head *list, struct list_head *head)
Desc: move list to before head

10 int list_is_singular(const struct list_head *head)
Desc: check head 是否只有一个 entry

11 void list_cut_position(struct list_head *list, struct list_head *head, struct list_head *enry)
Desc: 用 entry 将 head 切成两个链表

```

list_cut_position:

![list_cut](../../resource/list_cut.png)

```
12.1 void list_splice(const struct list_head *list, const struct list_head *head)
Desc: join list into head after head

12.2 void list_splice_tail(const list_head *list, const struct list_head *head)
Desc: join list into head before head
```

list_splice and list_splice_tail 

![list_splice](../../resource/list_splice.png)
