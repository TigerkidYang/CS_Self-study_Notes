# ArrayLists

## 基础训练

---

### **题 01**

从顺序表中删除具有最小值的元素（假设唯一），并由函数返回被删元素的值。
空出的位由最后一个元素填补，若顺序表为空，则显示出错信息并退出运行。

```c
typedef struct {
    ElemType data[size];
    int length;
} AList;

ElemType delMin (AList *L) {
    if (L==NULL || L->length == 0) {
        printf("Error: Empty List.\n");
        exit(1);
    }
    int currentMin = 0;
    for (int i = 0; i < L->length; i++) {
        if (L->data[i] < L->data[currentMin]) {
            currentMin = i;
        }
    }
    ElemType toReturn = L->data[currentMin];
    L->data[currentMin] = L->data[L->length - 1];
    L->length--;
    return toReturn;
}
```

---

### **题 02**

设计一个高效算法，将顺序表 L 的所有元素逆置，要求算法的空间复杂度为 O(1)。

```c
typedef struct {
    ElemType data[MAXSIZE];
    int length;
} AList;

void reverse (AList *L) {
    if (L==NULL || L->length <= 1) {
        return;
    }
    int r = L->length - 1;
    for (int i = 0; i < r; i++) {
        ElemType temp = L->data[i];
        L->data[i] = L->data[r];
        L->data[r] = temp;
        r--;
    }
}
```

---

### **题 06**

将两个有序顺序表合并为一个新的有序顺序表，并由函数返回结果顺序表。

```c
typedef struct {
    ElemType data[MAXSIZE];
    int length;
} AList;

AList mergeAList (AList *L1, AList *L2) {
    if (L1==NULL||L2==NULL) exit(1);
    AList result;
    result.length = 0;
    int m = 0;
    int n = 0;
    while (m < L1->length && n < L2->length) {
        if (L1->data[m]<=L2->data[n]) {
            result.data[m+n] = L1->data[m];
            m++;
        } else {
            result.data[m+n] = L2->data[n];
            n++;
        }
        result.length++;
    }
    if (m < L1->length) {
        for (int i = 0; i < L1->length - m; i++) {
            result.data[m+n+i] = L1->data[m+i];
            result.length++;
        }
    } else {
        for (int i = 0; i < L2->length - n; i++) {
            result.data[m+n+i] = L2->data[n+i];
            result.length++;
        }
    }
    return result;
}
```

写得更清楚：

```c
typedef struct {
    ElemType data[MAXSIZE];
    int length;
} AList;

AList mergeAList (AList *L1, AList *L2) {
    if (L1==NULL||L2==NULL) exit(1);
    if (L1->length + L2->length > MAXSIZE) exit(2);
    AList R;
    R.length = 0;
    int i = 0;
    int j = 0;
    while (i < L1->length && j < L2->length) {
        if (L1->data[i] <= L2->data[j]) {
            R.data[R.length++] = L1->data[i++];
        } else {
            R.data[R.length++] = L2->data[j++];
        }
    }
    while (i < L1->length) {
        R.data[R.length++] = L1->data[i++];
    }
    while (j < L2->length) {
        R.data[R.length++] = L2->data[j++];
    }
    return R;
}
```

---

### **题 09**

给定三个序列 A、B、C，长度均为 n，且均为无重复元素的递增序列，
请设计一个时间上尽可能高效的算法，逐行输出同时存在于这三个序列中的所有元素。

例如，数组 A 为 {1, 2, 3}，数组 B 为 {2, 3, 4}，数组 C 为 {-1, 0, 2}，则输出 2。

```c
typedef struct {
    ElemType data[n];
    int length;
} AList;

void inAll (AList *A, AList *B, AList *C) {
    if (A==NULL||B==NULL||C==NULL) return;
    if (A->length==0) return;
    int i = 0; int j =0; int k = 0;
    while (i < A->length && j < B->length && k < C->length) {
        if (A->data[i] == B->data[j] && A->data[i] == C->data[k]) {
            printf("%d\n", A->data[i]);
            i++; j++; k++;
        } else if (A->data[i] < B->data[j] || A->data[i] < C->data[k]) {
            i++;
        } else if (A->data[i] == B->data[j]) {
            k++;
        } else if (A->data[i] == C->data[k]) {
            j++;
        } else {
            j++; k++;
        }
    }
}
```

最小推进法：

```c
typedef struct {
    ElemType data[n];
    int length;
} AList;

void inAll (AList *A, AList *B, AList *C) {
    if (A==NULL||B==NULL||C==NULL) return;
    if (A->length==0) return;
    int i = 0; int j =0; int k = 0;
    while (i < A->length && j < B->length && k < C->length) {
        if (A->data[i] == B->data[j] && A->data[i] == C->data[k]) {
            printf("%d\n", A->data[i]);
            i++; j++; k++;
        } else {
            ElemType min = A->data[i];
            if (B->data[j]<=min) min = B->data[j];
            if (C->data[k]<=min) min = C->data[k];
            if (A->data[i]==min) i++;
            if (B->data[j]==min) j++;
            if (C->data[k]==min) k++;
        }
    }
}
```

## 真题训练

### 【2010 统考真题】

设将 $n$（$n > 1$）个整数存放到一维数组 $R$ 中。
设计一个在时间和空间两方面都尽可能高效的算法。
将 $R$ 中保存的序列循环左移 $p$（$0 < p < n$）个位置，即将 $R$ 中的数据由

$$
(X_0, X_1, \ldots, X_{n-1})
$$

变换为

$$
(X_p, X_{p+1}, \ldots, X_{n-1}, X_0, X_1, \ldots, X_{p-1})
$$

```c
typedef struct {
    ElemType data[MAXSIZE];
    int length;
} AList;

void leftP (AList *R, int p) {
    if (R==NULL||R->length<=1) return;
    for (int i = 0; i < p; i++) {
        R->data[R->length++] = R->data[i];
    }
    for (int k = 0; k < R->length - p; k++) {
        R->data[k] = R->data[k+p];
    }
    R->length = R->length - p;
}
```

问题： 1.依赖额外容量，MAXSIZE 不一定大于 n+p
2.p 没有规范化

三次逆置法：

```c
typedef struct {
    ElemType data[MAXSIZE];
    int length;
} AList;

void reversePart (AList *R, int start, int end) {
    if (start<0||end>=R->length) exit(1);
    while (start < end) {
        ElemType temp = R->data[start];
        R->data[start] = R->data[end];
        R->data[end] = temp;
        start++; end--;
    }
}

void leftP (AList *R, int p) {
    if (!R||R->length<=1) return;
    int n = R->length;
    if (p % n == 0) return;
    p = (p % n + n) % n;

    reversePart(R, 0, p-1);
    reversePart(R, p, R->length-1);
    reversePart(R, 0, R->length-1);
}
```

---

### 【2011 统考真题】

一个长度为 L（L≥1）的升序序列 S，处在第 ⌈L/2⌉ 个位置的数称为 S 的中位数。

例如，若序列

> S₁ = (11, 13, 15, 17, 19)，
> 则 S₁ 的中位数是 15。

两个序列的中位数是含它们所有元素的升序序列的中位数。

例如，若

> S₂ = (2, 4, 6, 8, 20)，
> 则 S₁ 和 S₂ 的中位数是 11。

现在有两个等长升序列 A 和 B，
试设计一个在时间和空间方面都尽可能高效的算法，找出两个序列 A 和 B 的中位数。

```c
typedef struct {
    ElemType data[MAXSIZE];
    int length;
} AList;

ElemType middleNumber (AList *A, AList *B) {
    if (A==NULL||B==NULL) exit(1);
    ElemType currentMax = A->data[0];
    int a = 0; int b = 0;
    while (a + b < A->length) {
        if (A->data[a] <= B->data[b]) {
            currentMax = A->data[a++];
        } else {
            currentMax = B->data[b++];
        }
    }
    return currentMax;
}
```

以上是直觉的归并做法，能达到$O(n)$，而以下的二分查找比较方法可以达到$O(\log n)$。

比较两个升序序列的二分之一处元素，大者已经确定比所有中的一半要大，因此所在序列可只取左半部分。同理，小者所在序列可只取右半部分。其中一个用完，另一个所选的即是所求。

```c
typedef struct {
    ElemType data[MAXSIZE];
    int length;
} AList;

ElemType middleNumber (AList *A, AList *B) {

}
```

---

当然可以，以下是你要求的 **数学公式使用 `$...$` 和 `$$...$$`** 的 Markdown 版本 👇：

---

### 【2013 统考真题】

已知一个整数序列 $A = (a_0, a_1, \dots, a_{n-1})$，其中 $0 \le a_i < n$（$0 \le i < n$）。
若存在 $a_{p1} = a_{p2} = \dots = a_{pm} = x$ 且 $m > n/2$（$0 \le p_k < n, 1 \le k \le m$），则称 $x$ 为 $A$ 的**主元素**。

例如：

- $A = (0, 5, 5, 3, 5, 7, 5, 5)$，则 $5$ 为主元素；
- $A = (0, 5, 5, 3, 5, 1, 5, 7)$，则 $A$ 中没有主元素。

假设 $A$ 中的 $n$ 个元素保存在一个一维数组中，设计一个尽可能高效的算法，找出 $A$ 的主元素。
若存在主元素，则输出该元素；否则输出 $-1$。

---

### 【2018 统考真题】

给定一个含 $n$（$n \ge 1$）个整数的数组，设计一个在时间上尽可能高效的算法，找出数组中**未出现的最小正整数**。

例如：

- 数组 ${-5, 3, 2, 3}$ 中未出现的最小正整数是 $1$；
- 数组 ${1, 2, 3}$ 中未出现的最小正整数是 $4$。

---

### 【2020 统考真题】

定义三元组 $(a, b, c)$（均为整数）的**距离**为：

$$
D = |a - b| + |b - c| + |c - a|
$$

给定 3 个**非空整数集合** $S_1, S_2, S_3$，按升序分别存储在 3 个数组中。
请设计一个尽可能高效的算法，计算并输出所有可能的三元组 $(a, b, c)$（$a \in S_1, b \in S_2, c \in S_3$）中的**最小距离**。

例如：

$$
S_1 = {-1, 0, 9}, \quad S_2 = {-25, -10, 10, 11}, \quad S_3 = {2, 9, 17, 30, 41}
$$

则最小距离的三元组为 $(9, 10, 9)$，最小距离为 $2$。

---
