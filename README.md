# Heap data structure implemented in C

## Follow the tutorial in [youtube but implemented in Java](https://www.youtube.com/watch?v=t0Cq6tVNRBA)

> For quick review

```c
#include <stdio.h>
#include <stdlib.h>
#include <limits.h>
#include <stdbool.h>

struct MinHeap {
    int capacity;
    int size;
    int *arr;
};

struct MinHeap *create_min_heap(int capacity) {
    struct MinHeap *min_heap = (struct MinHeap*)malloc(sizeof(struct MinHeap));

    min_heap->size = 0;
    min_heap->capacity = capacity;
    min_heap->arr = (int*)malloc(capacity * sizeof(int));

    return min_heap;
}

int g_left_child_idx(int par_idx) { return 2 * par_idx + 1; };
int g_right_child_idx(int par_idx) { return 2 * par_idx + 2; };
int g_parent_idx(int child_idx) { return (child_idx - 1) / 2; };

bool has_left_child(struct MinHeap *minHeap, int index) { return g_left_child_idx(index) < minHeap->size; };
bool has_right_child(struct MinHeap *minHeap, int index) { return g_right_child_idx(index) < minHeap->size; };
bool has_parent(int index) { return g_parent_idx(index) >= 0; };

int left_child(struct MinHeap *minHeap, int idx) { return minHeap->arr[g_left_child_idx(idx)]; };
int right_child(struct MinHeap *minHeap, int idx) { return minHeap->arr[g_right_child_idx(idx)]; };
int parent (struct MinHeap *minHeap, int idx) { return minHeap->arr[g_parent_idx(idx)]; };

void swap(int *a, int *b)
{
    int tmp = *a;
    *a = *b;
    *b = tmp;
};

/* Return the top, the minimum of the heap */
int peek(struct MinHeap *minHeap)
{
    return minHeap->arr[0];
};

void heaptify_down(struct MinHeap *minHeap)
{
    int idx = 0;
    while (has_left_child(minHeap, idx))
    {
        int smaller_child_idx = g_left_child_idx(idx);
        if (has_right_child(minHeap, idx) && right_child(minHeap, idx) < left_child(minHeap, idx))
        {
            smaller_child_idx = g_right_child_idx(idx);
        }
        if (minHeap->arr[idx] < minHeap->arr[smaller_child_idx])
            break;
        else swap(&minHeap->arr[idx], &minHeap->arr[smaller_child_idx]);
        idx = smaller_child_idx;
    }

};

void heaptify_up(struct MinHeap *minHeap)
{
    int idx = minHeap->size - 1;
    while (has_parent(idx) && parent(minHeap, idx) > minHeap->arr[idx])
    {
        int parent_idx = g_parent_idx(idx);
        swap(&minHeap->arr[parent_idx], &minHeap->arr[idx]);
        idx = g_parent_idx(idx);
    }

};

int poll(struct MinHeap *minHeap)
{
    int item = minHeap->arr[0];
    minHeap->arr[0] = minHeap->arr[minHeap->size - 1];
    minHeap->size--;

    heaptify_down(minHeap);

    return item;
};

void add(struct MinHeap *minHeap, int item)
{
    minHeap->arr[minHeap->size] = item;
    minHeap->size ++;

    heaptify_up(minHeap);
}

void print_heap(struct MinHeap *minHeap)
{
    int i = 0;
    while (i < minHeap->size)
    {
        printf("%d\n", minHeap->arr[i]);
        i++;
    }
}

int main(void)
{
    struct MinHeap *min_heap = create_min_heap(10);
    add(min_heap, 3);
    add(min_heap, 4);
    add(min_heap, 8);
    add(min_heap, 15);
    add(min_heap, 9);
    add(min_heap, 7);

    poll(min_heap);

    print_heap(min_heap);
    return 0;
};
```
