Note: Clients in this analysis are numbered according to the order in which they are given in the input.

Let's consider the 50% solution in order to develop some concepts that will help move us forward to the 100% solution.

We note that this problem is essentially greedy in nature. Suppose that we already know the value of M. Then, what we would like to do is 
determine whether, if client 1 rents the auditorium, we can still achieve this value. If the answer is yes, then we have determined that 
client 1 is part of the solution set, otherwise it is not. Each interval considered is either accepted or rejected, and then we move on to 
the next lowest-numbered interval that does not overlap any of the ones already accepted. (This check can, of course, be carried out in 
linear time.)

So we need to be able to answer the query: if some subset S of the intervals are chosen, what is the greatest number of other intervals 
(set S') we can choose so that no two overlap? This, too, can be solved with a greedy algorithm. We assume that that all the intervals 
are sorted, so that we know the order of elements in S and S'. We have to sort by ending time; the reason for this will be self-evident 
in the correctness of the algorithm. We sweep from left to right through S'. An interval is selected if it does not overlap with any 
interval already chosen. Note that all intervals in S are considered to have been chosen at the beginning. To check if an interval 
overlaps a previous one, we merely need to check if it starts before the previous one ends (remember, they're sorted on ending time!). 
Also, we need to avoid overlapping with an interval from S ahead, so we keep track of what the next interval in S is, walking through 
it as we walk through S'. Thus we can answer each query in linear time overall. We will only make up to one query for each of the 
intervals given: either we accept it or reject it on the basis of the answer. Thus we have a quadratic time algorithm. 
(In case you hadn't picked it up, running the query with S empty yields M.)

We need to speed this up to O(N lg N) to earn full marks. To do this, we will use the same general procedure, but improve our query 
time to logarithmic. That is, we determine the value of M first (we can do this in linear time after sorting, as discussed before), 
then we try to accommodate client 1, and then client 2, and so on. In summary:

  1. Sort the intervals given in input. (This time it actually doesn't matter if it's by beginning or ending time, but be consistent!)
  2. Determine the value of M.
  3. Assume that client 1 can be accommodated. What is the maximum total number of clients that can be accommodated now? Perform a query.
  4. If the total number from step 3 is M, then client 1 can indeed be accommodated, and is added to the solution set. 
    Otherwise client 1 is rejected.
  5. Find the smallest i such that client i has not yet been considered and client i's request interval does not overlap that of any 
    client already in the solution set. If a value of i is found, go back to step 3, using i this time instead of 1. Otherwise, we are 
    done; print out the solution set. (We could also simply check whether the solution set contains M intervals.)
    
All right, so steps 1, 2, and 4 are easy. Step 5 is not, perhaps, trivial, but is straightforward; we simply try increasing values of i 
until we find one that doesn't overlap. To check for overlap, a dynamic set (such as C++'s std::set) will suffice; find the earliest 
beginning time in the current solution set greater than or equal to the beginning time of interval i, and check if it is before 
interval i's ending time; also find the latest beginning time in the current solution set less than or equal to interval i's beginning
time, and check if the corresponding ending time is after interval i's beginning time. (This only works because the solution set 
consists of disjoint intervals; the general case of interval intersection is trickier.) And don't forget to update the set. 
(NB: The official solution suggests a segment tree for this query. This is overkill in C++ but is reasonable in Pascal, which lacks a
built-in dynamic set; after all, we do know all possible element and query values beforehand. BIT is also possible.)

Step 3 is the trickiest. The following observation is key:
If an interval completely contains another, the former can be removed without affecting the maximum number of clients that can be 
accommodated.
The reasoning is simple. No valid set of clients can contain both the former and the latter, but in any set containing the former, we 
can replace it with the latter without introducing any overlapping intervals. We will call an interval which completely contains 
another a dominating interval. This brings us to the next observation:
When all dominating intervals have been removed, the remaining intervals satisfy the property that an interval which starts earlier 
than another also ends earlier than it.
The reasoning is again simple; if the interval starting earlier does not also end earlier, then it completely contains the other 
interval. (Careful: if there are identical intervals given in the input, we must remove all but one of them.)

Unfortunately, we cannot simply ignore all the dominating intervals because the lexicographically smallest maximum client set which we 
seek might contain one of them. However, notice that removing them does not affect the number of clients that can be accommodated. 
This provides us with a possible way of carrying out step 3 that we will develop further. Let Σ denote the set of intervals from the 
input with all dominating intervals removed. Now, when we select interval 1 (which may or may not be an element of Σ), it partitions 
the intervals in Σ into three sets: ones which lie to the left of interval 1, those which lie to the right, and those which overlap.
Furthermore, if Σ is sorted, each of these three regions is contiguous. Suppose we can, in logarithmic time, determine the maximum 
number of intervals in some contiguous subset of Σ that can be selected without overlap. Then we can query the left of interval 1 and 
the right of interval 1 and add the results and add another one for interval 1, then see if the answer is M. Suppose now interval 1 is 
accepted and we test interval 2. It might seem that, since the two intervals together divide Σ into three regions where overlap does 
not occur, that we have to perform three queries, and that in general the number of queries increases with each new interval added to 
the solution set. But this is not so; when we add interval 2, if it lies on the left of interval 1, then the intervals to the right of 
interval 1 are unaffected, and performing another query on the intervals to the right is pointless. Indeed, adding an interval divides 
a previously existing segment into two, so we don't need to perform more than two queries per interval added.

The logarithmic time query is the crux of the solution. Given a contiguous subset of intervals in the sorted order of Σ, can we quickly 
determine how many non-overlapping ones can be selected? Can we do it in logarithmic time? It's easy to do it in linear time; start 
with the leftmost, then repeatedly choose the next leftmost that doesn't overlap the previous one until no more intervals can be chosen.
We will use essentially this technique, but speed it up to logarithmic time using a sparse table. For each interval i in Σ, define f(i)
to be the leftmost interval in Σ which is to the right of i and does not overlap i. Then, for some contiguous subsequence of Σ, to 
answer our query we can just determine how many times we can iterate the function f on i before we end up past the end of our 
subsequence. To this end, we number the intervals in Σ (so that the leftmost is 1', the next leftmost is 2', and so on) and we compute, 
for each i, the result of iterating f once, twice, four times, eight times, and so on up to N. Thus our table has O(N) rows and O(log N)
columns. The first column can be computed in linear time with a pointer walk; computing the rest of the table is trivial; constant time 
per entry gives O(N log N) time overall for the sparse table generation. Our query to the sparse table is the question of how many times 
we need to iterate f on interval i before we pass interval j (the right boundary of the subsequence of Σ). To answer this query,
we determine the bits of the answer from highest to lowest, essentially walking towards j in step sizen that are decreasing powers of 2,
using the sparse table data to perform each step in constant time, so the query takes logarithmic time overall.

So we should add a step 1.5, which is to precompute the sparse table. (Okay, it could just as well be 2.5, but the total amount of code
will be less if you use the sparse table to perform step 2.) Then step 3 can be expanded as follows:

  1. Determine where the interval being tested resides in relation to the solution set; that is, determine whether it is at the left end 
    of the solution set, the right end of the solution set, or between two intervals in the solution set. We use the same dynamic set 
    we use in step 5 for this (no point in maintaining more than one).
  2. Note that the intervals in the solution set split Σ up into contiguous subsequencen of intervals not overlapping the solution set, 
    and that introducing the new interval, in general, further splits one of these subsequencen into two smaller ones. 
    Use binary search to find the endpoints of these two subsequencen.
  3. Perform one query on each side, add the results, and add one (for the tested interval). Check if the answer is the same as the 
    result of the query on the subsequence before splitting. (Note that the latter value can simply be stored in the set somewhere.)
  
(A possible missing piece from this solution: how do we remove dominating intervals? It's not terribly difficult: sort them by starting 
time and sweep through with a stack. When we push an interval, we pop off all the intervals on the top of the stack that dominate it.)
