N can get pretty big, so copy+pasting the code will get you only a few points.

To get the real solution, let's look at what the solution is actually doing.

We can see that it's trying to find the biggest divisor of N (not including N itself).

Then just take N - (biggest divisor) to get counter.

Note that if we can find the smallest divisor (not counting 1), we've actually found the biggest divisor: just take N / x.

Finding the smallest divisor should be pretty easy: all we need is a for loop from 2 to sqrt(N).

(If there is no divisor ≤ sqrt(N), then N is prime and 1,N are the only divisors).

Code follows:

```cpp
int best = N; //worst case scenario
for (int i = 2; i * i ≤ N; i++)
	if (N % i == 0) {
		best = i;
		break;
	}

cout << N-(N/best) << endl;
```
