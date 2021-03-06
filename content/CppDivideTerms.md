# ([C++](Cpp.md)) [DivideTerms](CppDivideTerms.md)

[Math](CppMath.md) [code snippet](CppCodeSnippets.md) to divide two
sets of product terms. For example: for sets n and d, where n =
{1,2,3,4} and d = {1,2,3}, [DivideTerms](CppDivideTerms.md) returns
(1\*2\*3\*4)/(1\*2\*3)=4. This can be used to divide two
[factorials](CppFactorial.md).

```c++
#include <vector>

//From http://www.richelbilderbeek.nl/CppStripeAwayTerms.htm
void StripeAwayTerms(std::vector<int>& v_x, std::vector<int>& v_y)
{
  std::vector<int>::iterator x = v_x.begin();
  const std::vector<int>::iterator x_end = v_x.end();
  for ( ; x!=x_end; ++x)
  {
    if (*x == 1) continue;
    std::vector<int>::iterator y = v_y.begin();
    const std::vector<int>::iterator y_end = v_y.end();
    for ( ; y!=y_end; ++y)
    {
      if (*y == 1) continue;
      if (*x % *y == 0) { *x/=*y; *y=1; }
      if (*y % *x == 0) { *y/=*x; *x=1; }
    }
  }
}

#include <numeric>
#include <functional>
#include <cassert>

int DivideTerms(
  std::vector<int> terms_numerator,
  std::vector<int> terms_denominator)
{
  StripeAwayTerms(terms_numerator,terms_denominator);
  const int numerator
    = std::accumulate(
      terms_numerator.begin(),terms_numerator.end(),
      1,std::multiplies<int>());
  const int denominator
    = std::accumulate(
      terms_denominator.begin(),terms_denominator.end(),
      1,std::multiplies<int>());
  assert(numerator >= denominator
    && "Numerator value must be denominator value at least");
  assert(numerator % denominator == 0
    && "Dividing numerator and denominator must not result in a double");
  return numerator / denominator;
}
```

## [DivideTerms](CppDivideTerms.md) test

```c++
#include <functional>

//From http://www.richelbilderbeek.nl/CppFunctorIncrease.htm
struct Increase : public std::unary_function<void,int>
{
  explicit Increase(const int initValue = 0) : mValue(initValue) {}
  void operator()(int& anything)
  {
    anything = mValue;
    ++mValue;
  }
  private:
  int mValue;
};

#include <algorithm

//From http://www.richelbilderbeek.nl/CppGetFactorialTerms.htm
const std::vector<int> GetFactorialTerms(const int n)
{
  std::vector<int> v(n);
  std::for_each(v.begin(), v.end(),Increase(1));
  return v;
}

#include <cassert>

//Caution: for n>12 results might get incorrect
int Factorial(const int n)
{
  assert(n>=0);
  int result = 1;
  for (int i=1; i<=n; ++i)
  {
    result*=i;
  }
  return result;
}

#include <iostream>

int main()
{
  for (int i=0; i!=1000; ++i)
  {
    const int d = 0 + 1 + (std::rand() % 6);
    const int n = d + 1 + (std::rand() % 6); //n must max be 12
    assert(n <= 12 && "for n>12 Factorial might yield incorrect results");
    const int r = Factorial(n) / Factorial(d);
    std::cout << n << "! / " << d << "! = " << r << std::endl;
    const int r2 = DivideTerms(GetFactorialTerms(n),GetFactorialTerms(d));
    assert(r==r2);
  }
}
```
