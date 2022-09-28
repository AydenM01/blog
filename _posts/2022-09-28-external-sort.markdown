---
layout: post
title:  "CS 6422 External Sort"
date:   2022-09-28
categories: School
    
---    
My first assignment of Grad School.

### Overview

I am currently in my first semester of my MSCS at Georgia Tech, and am taking the following two courses:
- CS 6210: Advanced Operating Systems
- CS 6422: Database System Implementation

My first assignment of this semester was for CS 6422, and it was to implement an [External Sorting Algorithm](https://en.wikipedia.org/wiki/External_sorting) in C++.

### External Sorting
External Sorting is used when you wish to sort a list so large that the entire list cannot fit into the resident memory of your system. The concept of this was very interesting to me at first, as I always wondered how a DBMS such as MySQL could return an Order By query for millions of rows while maintaining the performance of an in-memory sorting algorithm, such as Quick Sort.

As it turns out, there are various methods of accomplishing this, but I opted to implement the External Merge Sort
algorithm due to its simplicity. Imagine the list you want to sort exists in a file (input_file), which can be very large. First, you need to decide what maximum size list in bytes you can fit into resident memory (max_mem). For every max_mem bytes in the file, we can read that chunk of bytes into memory, sort it using a standard in-memory sort (i.e qsort), and create a new temporary file housing that sorted chunk.

Now comes the fun part. Now that we have k sorted lists (chunks) in these temporary files, we can perform a k-way merge on these lists. Basically, we can instantiate a buffer of size k elements in memory, where each element represents the "current" element of its chunk (current starts at index 0), write the smallest element in the buffer to an output file, and read in the next element of the respective list. Once this k-way merge is completed, the entire externally-sorted list is in the output file! I personally made use of the std::priority_queue library to make reading the min element more efficient.

Now, the observant reader would have pointed out a potential problem with the above algorithm description. That is, what if the size k buffer cannot fit into memory? In that case, you could simply perform a two-way merge until the amount of resultant chunks (k') does fit into the memory buffer.

### Challenges
The biggest challenge for me during this assignment was getting familiar with C++, as I have never used it before. Since C++ is a superset of C and I had some C experience in the past, it was tempting to implement my solution using basic C semantics. However, the instructors strongly recommended using this project to get familiar with some advanced C++ topics such as Smart pointers and Move/Copy Semantics.

First, I implemented my solution using basic C semantics, and slowly changed my solution to use shared pointers instead of standard C pointers (std::shared_ptr\<T\> vs T*). Shared Pointers are a type of Smart Pointer that are used when there might be more than one owner of some allocated memory. Smart Pointers are useful because they keep a count of the amount of active references, and when that count goes to 0 (such as when the pointers get deallocated from the stack on function return), the memory is automatically freed.

In addition, I really began to appreciate the flexibility C++ gives you. You get unlimited freedom to choose from passing by value or by reference, allocating objects on the stack or heap, etc. (unlike Java). In addition, the standard libraries are easy to import, give you access to tons of functionality, and the ability to override operators allows you to easily use them with any custom objects or structs of your choosing. For example, I created a struct acting as a priority_queue node and was able to create my own > operator to allow easy comparision between nodes. Once the operator was defined, it instantly worked in the priority queue as I intended.

### Conclusion
Overall, I feel this was a great introduction assignment for the course as I definitely got comfortable with C++ fundamentals and got a taste of what it feels like to add a feature into a DBMS.

I am feeling enthusiastic about what the rest of the semester holds.
