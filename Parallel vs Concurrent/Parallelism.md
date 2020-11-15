20201113105439

# Parallelism



refers to a way of executing code in more that one computer core at once. 

[[20201113104359]] Concurrent tasks are often run in parallel to achive much better performance.

This increment in speed can also be applied to tasks that were not thought of as concurrent but whose data dependencies enable running parts of the algorithm independently of each other. Think of the QuickSort algorithm for sorting: at each step the list is divided in two parts, and each of them is sorted separately. In this case, the subsequent sorting of the two lists can be made in parallel.

