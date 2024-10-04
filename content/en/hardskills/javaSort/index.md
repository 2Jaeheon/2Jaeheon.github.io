---
title: 🗂️ Java sorting algorithms (how sort() works)
summary: How Arrays.sort() and Collections.sort() methods work in Java (Dual Pivot Quick Sort & Timed Sort)
date: 2024-08-25
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com)'
---

# This is for people like this
While using the sort methods in Java, I was curious about the algorithm used. I learned about various sorting methods in my undergraduate data structure algorithms course, but I didn't know which sort is used in Java and how it works, so I wrote this article to learn more about it. I hope you find this article helpful if you are curious about how sort works in Java.
<br><br>

# 자바의 정렬 메소드
자바의 정렬 메소드는 대표적으로 두 가지 있습니다.

# Sorting Methods in Java
There are two main sorting methods in Java.

1. Arrays.sort()
The Arrays class inherits from the Object class and contains various methods for manipulating and searching arrays. By default, sort() works in ascending order, and we'll cover the detailed algorithm directly below.

2. Collections.sort()
The Collections class similarly inherits from the Object class, which consists of only static methods that operate on collections or return collections. Collections contains polymorphic algorithms that operate on Collections.


<br><br>
# Sorting a primitive type array (Dual-pivot Quick Sort)
For Java's primitive type arrays, we use the Dual-pivot Quick Sort to sort the array.

Single Pivot Quick Sort is a sorting technique that pulls a pivot from the array and splits the array so that all elements left at the pivot are less than or equal to the pivot and all elements to the right of the pivot are greater than the pivot.

Dual-Pivot Quick Sort is a refinement of Pivot Quick Sort that improves on the traditional performance of Quick Sort by taking the two pivots at either end of the array and using them.

It works by separating the array into three parts, and then in the first part, all elements are less than the left pivot, in the second part, all elements are greater than or equal to the left pivot and less than or equal to the right pivot, and in the third part, all elements are greater than the right pivot. Then, as you can see in the bar below, it moves the two pivots to the appropriate positions and begins to recursively Quick Sort the three parts using this method. If you're interested in a visual representation of how this works, you might want to read the following article.

According to the official Java documentation, it provides the following performance
The Dual-Pivot Quick Sort algorithm delivers O(n log(n)) performance on many datasets where other quicksorts degrade to quadratic performance, and is generally recognized as faster than traditional (1-pivot) Quicksort implementations.

Reasons why Dual Pivot Quicksort improves performance
Improved cache performance: If the cost of the algorithm were just the number of comparisons and swaps, we could say that Dual Pivot Quick Sort is not better than Single Pivot Quick Sort, but since modern computers use cache, Dual Pivot Quick Sort often has better cache performance due to fewer cache misses due to its partitioning strategy.

<br><br>

# Sorting an array of object types (Tim Sort)
Arrays of object types are sorted using a sorting algorithm called Tim sort. Tim Sort was created in 2002 and introduced to Java in 2009.

TimSort is a hybrid sorting algorithm that combines the best of both merge and insertion sorts. It is designed to respond well to different characteristics of real-world data, and adjusts its strategy based on the characteristics of the input data.

TimSort splits the array into smaller parts, which we call runs. Runs can be between 32 and 64 in size. It finds naturally sorted parts and uses them as runs, and calculates the optimal run length based on the total array length.

Each run is then sorted with an insertion sort. Insertion sort is naturally efficient on small arrays, so it's a good fit for our run size. We then use Merge Sort to merge the sorted Runs into a larger Run. In this course, we'll use two optimization techniques, Galloping and Merge Balance.

Galloping: A technique used to select elements from a run in succession to speed up the merge.
Merge balancing: Merges runs of similar size to increase efficiency.

Unmerged runs are set aside in a stack, and the runs in the stack are kept to satisfy certain conditions (e.g., ascending order), and the process is repeated over and over again until all runs have been processed.

Translated with www.DeepL.com/Translator (free version)


If you're interested in learning more about how it works, you can check out the following links.
[Tim Sort의 원리](https://skerritt.blog/timsort/)<br>
[Tim Sort 네이버 D2 블로그](https://d2.naver.com/helloworld/0315536) 


# Why use Tim Sort vs. Dual-Pivot Quick Sort?
Tim Sort performs very well on arrays with existing internal structure, and is very fast while maintaining a stable sort. Its time complexity is O(n log n), the same as Merge Sort, a traditional efficient sorting method, but it actually performs better on real-world data with internal structure.

For primitive types, the operation is fast and simple, and the size is small and fixed, so memory usage is predictable. Object types can vary in size, memory usage is more complex, and comparison operations can be more complex and time-consuming. In these situations, Tim Sort is faster and manages memory more efficiently.

For the above reasons, Tim Sort and Dual-Pivot Quick Sort are used separately.

Should I always use the sort() method instead of other sorting methods?
Not always. Depending on your data, you should use different methods. Here are some examples of data and its categorization by size and characteristics


## Classification by Data Size
### Small Data Sets
Insertion Sort is effective.
It is suitable for small arrays with simple implementation and fast execution time.
<br>
### Medium-Sized Data Sets
Quick Sort is generally a good choice.
It has an average time complexity of O(NlogN) and is efficient in most cases.
<br>
## Large Data Sets
Merge Sort provides stable performance.
It guarantees a time complexity of O(NlogN) even in the worst case.
<br><br>
## Classification by Data Characteristics
### Nearly Sorted Data
Insertion Sort is very efficient.
It can quickly complete the sort by utilizing already sorted parts.
<br>
### Data with Many Duplicates
3-way Quick Sort, a variant of Quick Sort, is effective.
It can efficiently handle duplicate elements.
<br>
### Cases Requiring Stability
Merge Sort is suitable.
It maintains the relative order of elements with the same value.
<br>
### Environments with Memory Constraints
Heap Sort is good.
It can perform sorting with almost no additional memory usage.
<br><br>


# parallelSort (parallelSort())
Parallel sorting is a technique that utilizes multiple processors or cores to perform sorting operations simultaneously. It uses a parallel divide-and-conquer method to sort. In Java, we implement it through the Arrays.parallelSort() method.

Here's how it works

It recursively splits the array into subarrays.
These subarrays are sorted in parallel by multiple threads.
The sorted subarrays are merged to produce the final sorted array. So why is there a merge sort and why use it?
In general, you can see a performance difference of about 2x or more when the size of the array is greater than 10000. Also, parallelSort() utilizes the Fork/Join framework to take advantage of the power of multi-core processors. It uses multiple threads simultaneously to perform the sort, unlike regular sort(), which only uses a single thread.

<br><br>


# Finish
Now that we've covered each sort in Java theoretically, in the next article, we'll go into more detail about how Dual-Pivot Quick Sort and Tim Sort work in Java directly in code. Thank you for reading this long article.