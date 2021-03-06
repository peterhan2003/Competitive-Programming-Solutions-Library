One might be tempted to look up an algorithm on Google.

However, the problem statement subtlely hints at a solution with the emphasized area formula.

(This area is derived from the cross product, see Wikipedia if you're curious)

If a point is inside a triangle, the three triangles it makes with the sides of the triangle should add up to the big triangle.

Otherwise, the area of the three 'subtriangles' will be too big:


This leads to a very simple solution (with some macros to make life easier).

```cpp
#include <iostream>
using namespace std;

typedef pair<int,int> point;
#define X first
#define Y second
double area(point& a, point& b, point& c) // as given
{
	//precision is not an issue here, multiples of 0.5 are OK in binary
	return abs(a.X*(b.Y-c.Y) + b.X*(c.Y-a.Y) + c.X*(a.Y-b.Y)) / 2.0;
}

int main()
{
	point A, B, C;
	cin >> A.X >> A.Y;
	cin >> B.X >> B.Y;
	cin >> C.X >> C.Y;
	
	double triArea = area(A, B, C);
	cout.precision(1);
	cout << fixed << triArea << endl;
	
	int N; cin >> N;
	int count = 0;
	while (N--)
	{
		point P;
		cin >> P.X >> P.Y;
		if (area(P, A, B) + area(P, B, C) + area(P, A, C) == triArea)
			count++;
	}
	
	cout << count << endl;
}
```
