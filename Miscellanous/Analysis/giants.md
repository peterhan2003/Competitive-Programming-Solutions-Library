We will try to find some characteristic of a path that is common to all optimal paths, and use that to produce our answer.

First, note that all paths will pass through either 0, 1, 2, 3, or more endpoints of necks. If we determine that the optimal path passes 
through at least N endpoints, then we can just consider every combination of N points to determine our answer 
(if it actually passes through more than N endpoints, that will be a superset of at least one combination we try).

The optimal path cannot pass through 0 endpoints. If it does, we can always find a better path by decreasing Mikasa's starting altitude 
while preserving the slope of the path until it meets one endpoint. By doing so, we preserve the number of giants destroyed, while having 
a lower starting altitude.

The optimal path cannot pass through only 1 endpoint either. If it does, we can always find a better path by decreasing Mikasa's starting 
altitude while increasing the slope of the path, such that the new "rotated" path still passes through that point. We "rotate" the path 
this way until it meets another endpoint. By doing so, we preserve the number of giants destroyed, while having a lower starting altitude.

Since we have determined that the optimal path must pass through at least 2 points, we can try every pair of endpoints as points on a path 
and pick the best one. This leads to a O(G<sup>3</sup>) solution, well within the constraints.
