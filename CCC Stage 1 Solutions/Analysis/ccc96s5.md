
Due to the limitations on N, rather than brute forcing every pair X[i] and Y[j], 
we should only loop through the input once while keeping track of the max distance.

Basically, the idea is to have one for loop going through the values in X and a variable "j" keeping our current location at Y, 
both starting at 0.

For every X[i], we increase our location counter j until Y[j] is smaller than X[i], making sure j is always larger than 
(i + max distance found so far) and updating the max distance. If j ever goes out of bounds, we stop and output as a larger distance 
cannot be found.

For example:
If we are at the index 3 in X and the current max is four (from the first 8 in X to the 6 in Y), 
we immediately move the location at Y to (3 + 4 + 1) = 8 as any Y before Y[8] will yield a distance ≤ the current max which is of no use 
to us.

```
Index:  0  1  2  3  4  5  6  7  8

                 -
X |     8  8  4  4  4  3  3  3  1

Y |     9  9  8  8  6  5  5  4  3
```
