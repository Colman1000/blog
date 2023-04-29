---
title: "Data Structures: Linked Lists"
description: "Data Structures: Understanding Linked Lists"
date: 2023-04-29T15:43:13+01:00
tags: ["data-structure", "linked-lists"]
categories: ["Data Structures", "Arrays"]
draft: false
author: "@AI"
cover:
  image: "data-structures-linked-list.png"
  alt: "Data Structures: Linked Lists | Cover Image"
  caption: "Data Structures: Linked Lists"
images: ['data-structures-linked-list.png']
keywords: ["data-structure", "linked-lists"]
summary: "In this article, we deep dive into another popular data structure. The Linked List"
---

Linked Lists are an important data structure in computer science used to store a sequence of elements. Unlike arrays,
linked lists use a series of connected nodes to store data, making them dynamic in size and allowing for efficient
insertion and deletion of elements. In this article, we will discuss what linked lists are, when to use them, and how to
implement them in TypeScript.

### TL;DR

A linked list is a data structure that consists of a sequence of nodes, each containing data and a reference to the next
node, allowing for dynamic size and efficient insertion and deletion operations.

##### PROS

* **Dynamic Size**; Linked lists can grow or shrink dynamically, making them ideal for scenarios where the size of the
  data is unknown or may change over time.
* **Memory Efficiency**; Linked lists are memory efficient compared to arrays, especially when the size of the data is
  unknown or may change over time.
* **Flexibility**; Linked lists can be used to implement other data structures, such as stacks and queues.

##### CONS

* **Slower Than Arrays**; Linked lists are sequentially accessed, which can result in slower access times than arrays.
* **Uses More Memory Compared To Arrays**;Linked lists require additional memory to store pointers/references to the
  next node, which can result in higher memory usage than arrays.

### What is a Linked List?

A linked list is a collection of nodes, where each node contains a value and a reference to the next node in the
sequence. The first node is called the head and the last node is called the tail. A linked list can be either singly
linked, where each node only has a reference to the next node, or doubly linked, where each node has a reference to the
next and previous nodes.

Here is an example of a singly linked list:

```typescript
class ListNode {
    val: number;
    next: ListNode | null;

    constructor(val?: number, next?: ListNode | null) {
        this.val = (val === undefined ? 0 : val);
        this.next = (next === undefined ? null : next);
    }
}

const node1 = new ListNode(1);
const node2 = new ListNode(2);
const node3 = new ListNode(3);

node1.next = node2;
node2.next = node3;
```

In the example above, we create three nodes, `node1`, `node2`, and `node3`, with values of `1`, `2`, and `3`,
respectively. We then link them together by setting the next property of each node to the next node in the sequence.

### Advantages of Linked Lists

##### I. Dynamic size

* Linked lists can grow or shrink dynamically, making them ideal for scenarios where the size of the data is unknown or
  may change over time.
* This is in contrast to arrays, which have a fixed size.

##### II. Efficient insertion and deletion

* Linked lists offer efficient insertion and deletion operations, especially when compared to arrays.
* To insert or delete an element in an array, we may need to shift all the elements to the right or left of the
  insertion or deletion point, which can be time-consuming for large arrays.
* In contrast, with a linked list, we can simply update the pointers/references to insert or delete an element, which is
  much faster than shifting all the elements in an array.

##### III. Memory efficiency

* Linked lists are memory efficient compared to arrays, especially when the size of the data is unknown or may change
  over time.
* With arrays, we need to allocate memory for the entire array upfront, even if we don't know the exact size of the data
  at the time of allocation.
* In contrast, with linked lists, we can allocate memory as we add elements to the list, which can result in significant
  memory savings for large data sets.

##### IV. Flexibility

* Linked lists can be used to implement other data structures, such as stacks and queues.
* This is because linked lists offer efficient insertion and deletion operations, which are essential for implementing
  these data structures.

Overall, linked lists offer several advantages over other data structures, including dynamic size, efficient insertion
and deletion, memory efficiency, and flexibility. These advantages make linked lists an ideal data structure for
scenarios where the size of the data is unknown or may change over time, or where efficient insertion and deletion
operations are required.

### Disadvantages of Linked Lists

##### I. Slower access times than arrays

* Linked lists are sequentially accessed, which can result in slower access times than arrays.
* To access an element, we need to traverse the list from the beginning until we reach the desired element.

##### II. Higher memory usage than arrays

* Linked lists require additional memory to store pointers/references to the next node, which can result in higher
  memory usage than arrays.
* This can be especially problematic for large lists.

##### III. Inability to perform binary search operations

* Unlike arrays, linked lists do not support binary search operations.
* This means that searching for an element in a linked list requires traversing the list sequentially, which can be
  time-consuming for large lists.

##### IV. Traversing a singly-linked list in reverse requires additional storage and time complexity

* Singly-linked lists can only be traversed in one direction, so reversing the list requires additional storage and time
  complexity.
* To reverse a singly-linked list, we need to allocate a new node for each element in the list, which can be
  time-consuming and memory-intensive.

Overall, while linked lists offer several advantages over other data structures, they also have some disadvantages that
need to be taken into consideration when deciding which data structure to use for a particular scenario.

### When to use Linked Lists

Linked lists are often used in scenarios where a dynamic collection of elements is needed, or when efficient insertion
and deletion of elements is required. Some examples of when linked lists are commonly used include:

* Implementing stacks and queues
* Implementing hash tables
* Implementing graph algorithms
* Processing large datasets where memory is a concern

### Implementing Linked Lists in TypeScript

To implement a linked list in TypeScript, we can define a `ListNode` class with a value and a next property that points
to the `next` node in the sequence. We can then create a `LinkedList` class that has a `head` and `tail` property, and
methods for adding and removing nodes from the list.

Here is an example of a singly linked list implementation in TypeScript:

```typescript
class ListNode<T> {
    val: T;
    next: ListNode<T> | null;

    constructor(val: T, next: ListNode<T> | null = null) {
        this.val = val;
        this.next = next;
    }
}

class LinkedList<T> {
    head: ListNode<T> | null = null;
    tail: ListNode<T> | null = null;
    size: number = 0;

    add(val: T) {
        const node = new ListNode(val);
        if (!this.head) {
            this.head = node;
            this.tail = node;
        } else {
            this.tail!.next = node;
            this.tail = node;
        }
        this.size++;
    }

    remove(val: T) {
        let current = this
            .head;
        let previous: ListNode<T> | null = null;
        while (current) {
            if (current.val === val) {
                if (previous) {
                    previous.next = current.next;
                } else {
                    this.head = current.next;
                }
                if (!current.next) {
                    this.tail = previous;
                }
                this.size--;
                return true;
            }
            previous = current;
            current = current.next;
        }
        return false;
    }
}

const list = new LinkedList<number>();
list.add(1);
list.add(2);
list.add(3);
list.remove(2);
console.log(list); // { head: { val: 1, next: { val: 3, next: null } }, tail: { val: 3, next: null }, size: 2 }

```

In this example, we define a `ListNode` class that takes a value of type `T` and a reference to the next node. We then
define a `LinkedList` class with a `head`, `tail`, and `size` property. The `add` method adds a new node to the end of
the list, and the `remove` method removes a node with a given value from the list.

## Conclusion

Linked lists are a powerful data structure that can be used in a variety of scenarios where a dynamic collection of
elements is needed or when efficient insertion and deletion of elements is required. They offer several advantages over
other data structures, including dynamic size, efficient insertion and deletion, and memory efficiency. With the
examples and implementation provided in this article, you should now have a good understanding of what linked lists are,
when to use them, and how to implement them in TypeScript.

---

Thank You 🥳.

**Suggestions and Corrections are very welcome, Please comment below**
