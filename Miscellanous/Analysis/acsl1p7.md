First, let's discuss some "bad" solutions. This problem is exactly the kind one would have expected to appear on an ACSL contest at the 
time that this problem was created (for use in an ACSL practice), in that it invites solutions that require relatively little algorithmic 
insight.

It is actually possible to solve this problem by hardcoding. Ignoring rotations and reflections, there are 35 distinct hexominoes, 11 of 
which are cube nets. The lists can be found online, but during the actual contest, of course, one would have to find the valid cube nets 
by hand. So one merely needs to hardcode all 11 valid cube nets into the program, and then, for each possible input, test all possible 
translations, rotations, and reflections in an attempt to match it against one of the valid cube nets.

The problem can also be solved by doing a difficult simulation in which we actually attempt to construct the cube. This approach is not 
recommended, as it would be rather tricky to get right during a contest.

There is actually a "nice" solution, though. The idea is to attempt to find a single "candidate opposite" for each of the squares in the 
net. To do so, we start at that square, take one step in one of the four directions, then turn 90 degrees, continue walking for 0 or more 
steps, and then turn 90 degrees again so that we are facing our original direction, and take another step forward. If our final square is 
part of the net, as well as all intermediate squares traversed, then our final square is a candidate opposite. A net is a valid cube net
if and only if every square in the net has exactly one candidate opposite.

For example, in the sample case, the square 1 has exactly one candidate opposite: walk down one step, left or right 0 steps, and then 
down one more step, to arrive at the 4. The square 4 likewise has 1 as its only candidate opposite. It is easy to see that whenever 
there are three squares in a row in the net, its two ends are candidate opposites of each other. In this way 5 and 3 match each other, 
as do 2 and 6.

However, in the straight hexomino, which might be represented
```
1 2 3 4 5 6
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
```
the square 3 has two candidate opposites (1 and 5), as does 4 (2 and 6). So this is not a valid cube net. Likewise, in a net such as

```
1 2 3 0 0 0
4 5 6 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0

```
the square 2 has no candidate opposite. If we walk left first, then no matter how many steps we take up or down, we won't be able to 
walk left again; similarly we can't walk up, down, or right either as a first step, without leaving the net at some point. 
(The square 5 also has no candidate opposite.) So this is not a valid cube net, either.

It is not too hard to see why this algorithm works. A valid cube net must be connected, so there must be a path on the net from the 
initial square to its actual opposite. If we imagine an actual cube, a simple path from a face to its opposite involves choosing some 
direction, taking "one step" in that direction (to reach one of 4 possible faces), then turning 90 degrees, continuing to walk some 
(possibly zero) number of steps, then finally stepping onto the destination face (the opposite). This corresponds to exactly the kind of
path our algorithm is looking for. If we don't manage to find any such path, then when we attempt to fold up our cube net, we will not
get a cube because the face will have nothing opposite it. If we find more than one such path, then when we attempt to fold up our cube 
net, there will be more than one square trying to occupy the position opposite the initial square.
