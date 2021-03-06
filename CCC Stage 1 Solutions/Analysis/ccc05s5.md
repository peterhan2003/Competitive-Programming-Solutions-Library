There are a number of ways to solve this problem. First note that we would like a data structure which allows us to
1. insert,
2. query the rank of the newly inserted element.

1. By re-sorting the data every time a new element is added. Unfortunately, this takes O(n log n) time per element, or O(n<sup>2</sup> log n) time, 
and even radix sort gives O(n<sup>2</sup>) time (O(n) time per element)- too slow.

2. By using an O(n) insertion per element, as with insertion sort. This takes O(n<sup>2</sup>) time in the worst case, so it times out. 
However, the time required can be cut by a factor of four by inserting from either the front or the back (whichever requires fewer 
operations) by using a buffer that extends in both directions; while still O(n<sup>2</sup>) in the worst case, the judge computer is fast enough to 
allow this solution to pass (and the average-case complexity is much improved).
Note that this solution would probably fail the worst case during the actual CCC contest - however, it would certainly get more partial 
marks than solution 1, if implemented correctly. See dAedaL's code for an implementation.

3. In analogy with #2, by the use of a list. Theoretically this allows faster insertions (constant time) but takes longer to traverse. 
The author has not tested whether or not such a solution will pass in time.

4. By using a balanced binary search tree. The built-in containers set and map will not do, because they do not support the query 
operation. There are a number of options: randomized tree, splay tree, AVL tree, and red-black tree are the most well-known. 
Skip lists will also work. The runtime of this solution is O(n log n), but it could be difficult to get right on the CCC, with its time 
limit of 3 hours.

5. By using a binary indexed tree. The data is sorted first so that coordinate compression may be applied - that is, the values are 
transformed to 0, 1, ... n-1 such that the answer remains the same. For example, the sample test case would give 3, 0, 2, 1, 4. 
Then, they are inserted beginning with the first one and ending with the last one, into a binary indexed tree. All entries are initially 
zeroes; each insert operation involves setting the corresponding element to a one, and a query is done by querying the range from the 
beginning up to the element to be inserted for the sum. This solution is the easiest to code and significantly faster than solutions #1-#4.

6. All the above solutions involve some way of calculating all the ranks and then adding them together - but the question doesn't ask for 
these, it only asks for the average rank. An even faster approach, due to Sean Henderson, former PEG member, is to note that the answer is 
simply i/n + 1, where i is the number of inversions in the input data. This is because each time we insert a new element, 
the number of inversions that it contributes is equal to the number of elements before it with lower scores - except we add 1 to the 
average because we have to add 1 for each individual insertion.
The number of inversions can be calculated in O(n log n) time using a divide-and-conquer algorithm. 
The number of inversions in a list is equal to the the number of inversions in the first half plus the number of inversions in the 
second half plus the number of times that an element in the second half is smaller than an element in the first half. To calculate this 
latter quantity, we use mergesort on the two halves - each time we take an element from the second half, it is smaller than or equal to 
the elements left over from the first list. For an implementation, see Sean's code posted on the MMHS website. It is written in Java, 
but uses no features of the language that are not also present in C++.
