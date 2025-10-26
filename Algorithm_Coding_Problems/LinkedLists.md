# LinkedLists

## 基础训练

### **01.**

在带头结点的单链表 L 中，删除所有值为 x 的结点，并释放其空间。
假设值为 x 的结点不唯一，试编写算法以实现上述操作。

```c
typedef struct node {
    int data;
    node *next;
} node;

typedef struct SList {
    node *head;
    int length;
} SList;

void delValue (SList *L, int x) {
    if (!L||!L->head) return;
    node *p = L->head;
    while (p->next) {
        if (p->next->data == x) {
            node *temp = p->next;
            p->next = temp->next;
            free(temp);
            L->length--;
        } else {
            p = p->next;
        }
    }
}
```

---

### **03.**

试编写算法将带头结点的单链表**就地逆置**，
所谓“就地”是指辅助空间复杂度为 O(1)。

```c
typedef struct node {
    int data;
    node *next;
} node;

typedef struct SList {
    node *head;
    int length;
} SList;

void inPlaceReverse (SList *L) {
    if (!L || !L->head || !L->head->next || !L->head->next->next) return;
    node *pre = L->head->next;
    node *cur = pre->next;
    node *next = cur->next;
    pre->next = NULL;
    while (next) {
        cur->next = pre;
        pre = cur;
        cur = next;
        next = next->next;
    }
    cur->next = pre;
    L->head->next = cur;
}
```

---

### **06.**

设
C = { a₁, b₁, a₂, b₂, …, aₙ, bₙ }
为线性表，采用带头结点的单链表存放。
设计一个**就地算法**，将其拆分为两个线性表，使
A = { a₁, a₂, …, aₙ }，
B = { bₙ, …, b₂, b₁ }。

```c
typedef struct node {
    int data;
    node *next;
} node;

typedef struct SList {
    node *head;
    int length;
} SList;

void divedeTwo(SList *L, SList *A, SList *B) {
    if (!L||!L->head) return;
    A->head = L->head;
    B->head = (node*)malloc(sizeof(node));
    B->head->next = NULL;
    node *a = A->head->next;
    node *b = a->next;
    while (b) {
        a->next = b->next;
        a = a->next;
        b->next = B->head->next;
        B->head->next = b;
        b = a->next;
    }
}
```

---

### **07.**

在一个递增有序的单链表中，存在重复的元素。
设计算法**删除重复的元素**，
例如 (7, 10, 10, 21, 30, 42, 42, 51, 70)
将变为 (7, 10, 21, 30, 42, 51, 70)。

---

### **15.**

单链表有环，是指单链表的最后一个结点的指针指向了链表中的某个结点
（通常单链表的最后一个结点的指针域为空）。
试编写算法判断单链表是否存在环。

要求：

1. 给出算法的基本设计思想；
2. 根据设计思想，采用 C 或 C++ 语言描述算法，关键之处给出注释；
3. 说明所设计算法的时间复杂度和空间复杂度。

---

## 历年真题

---

### **17.（2009 年统考真题）**

已知一个带有表头结点的单链表，其结点结构为：

```
data | link
```

假设该链表只给出了头指针 list。
在**不改变链表结构**的前提下，请设计一个尽可能高效的算法，
查找链表中**倒数第 k 个位置上的结点**（k 为正整数）。
若查找成功，算法输出该结点的 data 域的值，并返回 1；
否则返回 0。

要求：

1. 描述算法的基本设计思想；
2. 描述算法的详细实现步骤；
3. 根据设计思想和实现步骤，采用程序设计语言描述算法（使用 C、C++ 或 Java 语言），关键之处给出简要注释。

---

### **18.（2012 年统考真题）**

假定采用带头结点的单链表保存单词。
当两个单词有相同的后缀时，可以共享相同的后缀存储单元，
例如 “loading” 和 “being” 的存储映像如下图所示（略）：

设 str1 和 str2 分别指向两个单词所在单链表的头结点，
链表结点结构为：

```
data | next
```

请设计一个**尽可能高效的算法**，找出 str1 和 str2 所指单词链表的**共同后缀的起始位置 p**。

要求：

1. 给出算法的基本设计思想；
2. 根据设计思想，采用 C 或 C++ 或 Java 语言描述算法，关键之处给出注释；
3. 说明你所设计算法的时间复杂度。

---

### **19.（2015 年统考真题）**

用单链表存储 m 个整数，结点结构为：

```
[data] [link]
```

且 |data| ≤ n（n 为正整数）。
现要求设计一个时间复杂度尽可能高的算法，
对链表中 data 的**绝对值相等的结点，仅保留第一次出现的结点**，删除其余绝对值相等的结点。

例如，给定的单链表 head 如下：

```
head → 21 → -15 → 15 → -7
```

删除后的 head 为：

```
head → 21 → -15 → -7
```

要求：

1. 给出算法的基本设计思想；
2. 使用 C 或 C++ 语言，给出单链表结点的数据类型定义；
3. 根据设计思想，采用 C 或 C++ 语言描述算法，关键之处给出注释；
4. 说明你所设计算法的时间复杂度。

---

### **20.（2019 年统考真题）**

设线性表
L = (a₁, a₂, a₃, …, aₙ₋₂, aₙ₋₁, aₙ)
采用带头结点的单链表存储，结点结构为：

```c
typedef struct node {
    int data;
    struct node *next;
} NODE;
```

请设计一个**尽可能高效且空间复杂度为 O(1)** 的算法，
重新排列 L 中的各结点，使得：

```
L = (a₁, aₙ, a₂, aₙ₋₁, a₃, aₙ₋₂, …)
```

要求：

1. 给出算法的基本设计思想；
2. 根据设计思想，采用 C 或 C++ 语言描述算法，关键之处给出注释；
3. 说明你所设计算法的时间复杂度。
