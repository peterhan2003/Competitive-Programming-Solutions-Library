This is a simple math problem dealing with weighted averages. In this case, we want to solve the inequality 
round(P*(100% − W%) + M*W%) ≥ Q, where M is the minimum possible integer mark on the exam. So in sample 1, if her mark is 85%, 
the exam is worth 30%, and she wants a 90%, then we notice that 85%*0.7 + 100%*0.3 = 89.5%, which rounds up to 90.

A simple way to solve this is by observing that the question limits the answer to an integer percentage. So, we can simple 
loop from 0 to 100 and try all possible values of M to see if they round to a value of Q or above. In a timed contest, 
this idea is the simplest and leaves the smallest room for error. The following source code implements this idea.

```cpp
#include <iostream>
#include <cstdio>
using namespace std;

int P, Q, W;

int main() {
  cin >> P >> Q >> W;
  for (int M = 0; M <= 100; M++) {
    double mark = 0.01*(P*(100 - W) + M*W);
    if ((int)(mark + 0.5) >= Q) {
      cout << M << endl;
      return 0;
    }
  }
  cout << "DROP THE COURSE" << endl;
  return 0;
}
```
