# Efficiency of the STL priority_queue

[Efficiency of the STL priority_queue](https://stackoverflow.com/questions/2974470/efficiency-of-the-stl-priority-queue)

-----------------

Priority_queue is a container adaptor, meaning that it is implemented on top of 
some underlying container type. By default that underlying type is vector, but a 
different type may be selected explicitly.

Priority queues are a standard concept, and can be implemented in many different 
ways; Some implementations use heaps. The complexities of single-element insert 
and erase are sequence dependent.

The priority_queue uses a vector (by default) as a heap, whichsupports random 
access to elements, constant time insertion and removal of elements at the end, 
and linear time insertion and removal of elements at the beginning or in the middle.

The priority queue adaptor uses the standard library heap algorithms to build and 
access the queue - it's the complexity of those algorithms you should be looking up 
in the documentation.

The top() operation is obviously O(1) but presumably you want to pop() the heap 
after calling it which 
(according to [Josuttis](http://www.josuttis.com/libbook/index.html)) is 
O(2*log(N)) and push() is O(log(N)) - same source.

And from the C++ Standard, 25.6.3.1, push_heap :
> Complexity: At most log(last - first) comparisons.

and pop_heap:
> Complexity: At most 2 * log(last - first) comparisons.

