---
draft: true
tags:
- C++
- Memory Allocation
- Best Practices
title: 'Memory Allocation in C++: Techniques and Best Practices'
---

## Introduction

Memory allocation is a crucial aspect of programming in C++. It is the process of reserving a block of memory for use in a program. In C++, there are different techniques for memory allocation, and choosing the right one can have a significant impact on the performance of your program.

In this article, we will explore the different techniques of memory allocation in C++, their advantages and disadvantages, and best practices for memory management.

## Stack and Heap Memory

In C++, there are two main types of memory allocation: stack and heap. 

### Stack Memory

Stack memory is a block of memory that is automatically allocated when a function is called. It is a limited resource, and its size is determined at compile time. Stack memory is used for storing local variables, function arguments, and return addresses.

Stack memory is fast because it is managed by the compiler and is allocated and deallocated automatically. However, it has limited size, and if the program requires more memory than the available stack space, it will crash.

### Heap Memory

Heap memory is a block of memory that is allocated dynamically at runtime. It is not managed by the compiler, and its size is not limited. Heap memory is used for storing objects that have a longer lifespan than local variables and function arguments.

Heap memory is slower than stack memory because it requires the use of the operating system's memory management routines. However, it is more flexible and can be used to allocate and deallocate memory on demand.

## Memory Allocation Techniques

In C++, there are several techniques for memory allocation, each with its own advantages and disadvantages.

### Stack Allocation

Stack allocation is the simplest technique for memory allocation in C++. When you declare a variable in a function, the compiler automatically allocates memory on the stack. The memory is automatically deallocated when the function returns.

```c++
void func() {
  int x; // memory is allocated on the stack
}
```

Stack allocation is fast and efficient, but it has limited size, and it is not suitable for large objects or data structures. It also requires that the size of the memory allocation be known at compile time.

### Heap Allocation

Heap allocation is a technique for allocating memory dynamically at runtime. In C++, you can use the `new` operator to allocate memory on the heap.

```c++
int* ptr = new int; // allocate memory on the heap
```

Heap allocation is more flexible than stack allocation because it can allocate memory on demand. However, it is slower and more error-prone than stack allocation because you must manually deallocate the memory when it is no longer needed.

```c++
delete ptr; // deallocate memory
```

If you forget to deallocate memory on the heap, you will have a memory leak, which can cause your program to consume an excessive amount of memory and eventually crash.

### Placement New

Placement new is a variant of the new operator that allows you to allocate memory for an object in a specific location in memory.

```c++
void* buffer = malloc(sizeof(MyObject));
MyObject* obj = new(buffer) MyObject();
```

Placement new is useful when you need to allocate memory for an object in a specific location, such as a memory-mapped file or a shared memory region.

### Smart Pointers

Smart pointers are objects that manage the lifetime of dynamically allocated objects. They automatically deallocate memory when it is no longer needed, preventing memory leaks.

```c++
std::unique_ptr<int> ptr = std::make_unique<int>();
```

Smart pointers come in several flavors
and provide a convenient way to manage memory without worrying about manual memory management. Here are the three most commonly used smart pointers in C++:

#### Unique Pointers

Unique pointers are smart pointers that own the object they point to. Only one unique pointer can point to an object at any given time, and when the unique pointer is destroyed, the object it points to is automatically destroyed as well.

```c++
std::unique_ptr<int> ptr = std::make_unique<int>();
```

Unique pointers are useful when you need to ensure that an object is destroyed when it is no longer needed.

#### Shared Pointers

Shared pointers are smart pointers that allow multiple pointers to point to the same object. The object is destroyed when the last shared pointer that references it is destroyed.

```c++
std::shared_ptr<int> ptr1 = std::make_shared<int>();
std::shared_ptr<int> ptr2 = ptr1;
```

Shared pointers are useful when you need to share ownership of an object between multiple parts of your program.

#### Weak Pointers

Weak pointers are smart pointers that do not own the object they point to. They are used in conjunction with shared pointers to avoid circular references.

```c++
std::weak_ptr<int> weakPtr;
{
    std::shared_ptr<int> sharedPtr = std::make_shared<int>();
    weakPtr = sharedPtr;
}
```

Weak pointers are useful when you need to reference an object without owning it.

### Memory Pools

Memory pools are a technique for allocating and deallocating memory in batches, rather than individually. Memory pools can improve performance by reducing the overhead of individual memory allocations and deallocations.

```c++
class MemoryPool {
public:
    MemoryPool(std::size_t blockSize, std::size_t numBlocks) {
        // allocate memory in a contiguous block
        block = new char[blockSize * numBlocks];
        // initialize the linked list of free blocks
        for (int i = 0; i < numBlocks; ++i) {
            char* blockPtr = block + i * blockSize;
            char** nextPtr = reinterpret_cast<char**>(blockPtr);
            *nextPtr = blockPtr + blockSize;
        }
        // set the last block's next pointer to null
        char** lastNextPtr = reinterpret_cast<char**>(block + (numBlocks - 1) * blockSize);
        *lastNextPtr = nullptr;
        // set the free block pointer to the first block
        freeBlock = block;
    }

    void* allocate(std::size_t size) {
        if (size > blockSize) {
            throw std::bad_alloc();
        }
        if (!freeBlock) {
            throw std::bad_alloc();
        }
        void* blockPtr = freeBlock;
        freeBlock = *reinterpret_cast<char**>(freeBlock);
        return blockPtr;
    }

    void deallocate(void* ptr) {
        char** nextPtr = reinterpret_cast<char**>(ptr);
        *nextPtr = freeBlock;
        freeBlock = static_cast<char*>(ptr);
    }

    ~MemoryPool() {
        delete[] block;
    }

private:
    char* block;
    char* freeBlock;
    std::size_t blockSize;
};
```

Memory pools are useful when you need to allocate and deallocate memory frequently, such as in a game or a real-time system.

## Best Practices

Now that we've explored the different techniques of memory allocation in C++, let's review some best practices for memory management.

### Use Stack Allocation for Small Objects

For small objects, it's best to use stack allocation whenever possible. Stack allocation is fast and efficient, and it doesn't require manual memory management.

### Use Heap Allocation for Large Objects

For large objects, it's
best to use heap allocation. Heap allocation can handle objects that are too large to fit on the stack, and it allows for more flexible memory management.

### Use Smart Pointers

Whenever possible, use smart pointers to manage memory. Smart pointers provide automatic memory management and can help prevent memory leaks and dangling pointers.

### Use Memory Pools

If you need to allocate and deallocate memory frequently, consider using memory pools. Memory pools can improve performance by reducing the overhead of individual memory allocations and deallocations.

### Avoid Raw Pointers

Avoid using raw pointers whenever possible. Raw pointers require manual memory management and can easily lead to memory leaks and dangling pointers.

### Avoid Circular References

Avoid circular references between objects. Circular references can cause memory leaks and make it difficult to manage memory.

### Use Memory Profilers

Use memory profilers to identify memory leaks and other memory-related issues in your code. Memory profilers can help you identify and fix memory-related issues before they become serious problems.

## Conclusion

Memory management is an important aspect of software development, and it's critical to understand the different techniques for allocating and deallocating memory in C++. Stack allocation, heap allocation, smart pointers, and memory pools are all important tools for managing memory in C++. By following best practices for memory management and using tools like memory profilers, you can ensure that your code is efficient, reliable, and free from memory-related issues.

## References

- Bjarne Stroustrup. The C++ Programming Language. Addison-Wesley Professional, 4th edition, 2013.
- Scott Meyers. Effective Modern C++. O'Reilly Media, 1st edition, 2014.
- Herb Sutter and Andrei Alexandrescu. C++ Coding Standards: 101 Rules, Guidelines, and Best Practices. Addison-Wesley Professional, 1st edition, 2004.