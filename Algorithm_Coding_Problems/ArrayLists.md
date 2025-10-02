# ArrayLists

## Problem 1

**Please design an algorithm that reverse an ArrayList L, with $O(1)$ space.**

```c
typedef int ElemType;

typedef struct {
    ElemType *data;
    int length;
} ArrayList;

void reverseAList(ArrayList *L) {

    if (L == NULL || L->length <= 1) {
        return;
    }

    int l = 0;
    int r = L->length - 1;
    ElemType temp;

    while (l < r) {
        temp = L->data[l];
        L->data[l] = L->data[r];
        L->data[r] = temp;
        l++;
        r--;
    }
}
```

### Double Pointers

The big idea is to use two pointers to do a lot of things. Like here, we do exchange from both directions. In following problems, we might also use two pointers but in other ways.

## Problem 2

**For an ArrayList L with length n, please design an algorithm deleting every element of value x, which has $O(n)$ time complexity and $O(1)$ space complexity.**

```c
typedef int ElemType;

typedef struct {
    ElemType *data;
    int length;
} AList;

void deleteX (AList *L, ElemType x) {

    if (L == NULL || L->length == 0) { return; }

    int s = 0;
    
    for (int f = 0; f < L->length; f++) {
        if (L->data[f] != x) {
            L->data[s] = L->data[f];
            s++;
        } else {
            continue;
        }
    }

    L->length = s;
}
```

### Double Pointers (Fast and Slow)

You see this time we do it in another way.

The slow one keep track on the position where the next no x value that the fast one find should be put.

## Problem 3

**Merge two sorted ArrayLists into one new sorted Arraylist and return it.**

```c
typedef int ElemType;

typedef struct {
    ElemType *data;
    int length;
} AList;

(*AList) mergeAlists (AList *L1, AList *L2) {

    if (L1 == NULL || L2 == NULL || L1->length == 0 || L2->length == 0) {return;}

    int l1 = 0;
    int l2 = 0;
    int l = 0;
    (*AList) L;

    while (l1 < L1->length || l2 < L2->length) {
        if (L1->data[l1] <= L2->data[l2]) {
            L->data[l] = L1->data[l1];
            l1++;
        } else {
            L->data[l] = L2->data[l2];
            l2++;
        }
        l++;
    }
    L->length = l;

    return L;
}
```