Since a right triangle has exactly one right angle, we can loop through all points in the set, counting, for each one of them, 

how many right triangles can be formed with that point at the right angle, and add all of the results to find the total number of 

right triangles.

To find the number of ways a right angle can be formed at a given point, we perform an angle sweep. We first sort all other points by 

their angular position around the central point. Then we loop through all possible choices for the second point. Once we have chosen the 

second point, we know the third point must be 90 degrees counterclockwise from it (note that by making sure the third point is always 

counterclockwise from the second, we avoid double-counting right triangles). Since the list of points is sorted by angle, we can 

binary search for the first and the last point located at this position, giving the number of possible choices for the third point. 

Adding all these numbers gives the total number of right triangles with the right triangle at the central point.

The complexity of this solution is O(N<sup>2</sup> log N), because O(N log N) time is spent on each iteration of the loop 

(O(N log N) to sort and O(log N) on each of 2N binary searches). See bbi5291's solution for an implementation. 

(Notice that a tolerance of 10<sup>-12</sup> gives the wrong answer, but 10<sup>-15</sup> works.) It is possible to avoid the binary searching by 

implementing a pointer walk instead, which reduces the running time of the inner loop to linear, but this does not affect the overall 

complexity because of the sorting step.
