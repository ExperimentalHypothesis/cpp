# ([C++](Cpp.md)) [AreMeansSame](CppAreMeansSame.md)

[AreMeansSame](CppAreMeansSame.md) is a [Math](CppMath.md) [code
snippet](CppCodeSnippets.md) to test if two means are the same,
assuming both distributions are normally distributed, on the same data
as [1].

Adapted from [the Boost
documentation](http://www.boost.org/doc/libs/1_42_0/libs/math/doc/sf_and_dist/html/math_toolkit/dist/stat_tut/weg/st_eg/two_sample_students_t.html).

```c++
#include <boost/math/distributions/students_t.hpp>

//From www.richelbilderbeek.nl/CppAreMeansSame.htm
//
// mean1 - mean2
// t = ----------------------------------------------------
// SQRT (pooled_variance * ( (1.0/size1) + (1.0/size2) )
//
// Difference_due_to_chance = t * 2.0
//
// ((size1-1)*stddev1*stddev1) + ((size2-1)*stddev2*stddev2)
// Pooled_variance = --------------------------------------------------------- )
// size1 + size2 - 2
// Degrees of freedom = size1 + size2 - 2
//
bool AreMeansSame(
  const double mean1,
  const double stddev1,
  const std::size_t size1,
  const double mean2,
  const double stddev2,
  const std::size_t size2,
  const double significance_level)
{
  const double degrees_of_freedom = static_cast<double>(size1 + size2 - 2);
  const double pooled_variance = ((static_cast<double>(size1-1) * stddev1 * stddev1)   + (static_cast<double>(size2-1) * stddev2 * stddev2))   / degrees_of_freedom;
  const double t_statistic = std::fabs(mean1 - mean2) / std::sqrt(pooled_variance * (     (1.0 / static_cast<double>(size1))   + (1.0 / static_cast<double>(size2)) ) );
  //std::cout << "t_statistic: " << t_statistic << '\n';
  const boost::math::students_t distribution(degrees_of_freedom);
   const double critical_value = boost::math::quantile(boost::math::complement(distribution,     significance_level / 2.0));
  //std::cout << "critical_value: " << critical_value << '\n';
  return (t_statistic < critical_value);
}

#include <iostream>

int main()
{
  //Values from
  // * David Heath. An introduction to experimental
  // design and statistics for biology. 1995.
  // ISBN: 1-85728-132-2 PB. Page 246, box 9.12:
  // The t test for difference in means of two populations
  // using small samples: mean activty of
  // acid phosphatase at pH3 and pH5
  if (AreMeansSame(11.24,1.26,5,14.12,1.45,5,0.05))
  { std::cout << "It cannot be rejected that means are the same\n";
  }
  else
  { std::cout << "It can be rejected that means are the same\n";
  }
}
```

[References](CppReferences.md)
-------------------------------

 

 * [1] David Heath. An introduction to experimental design and statistics for biology. 1995. ISBN: 1-85728-132-2 PB. Page 246, box 9.12: The t test for difference in means of two populations using small samples: mean activty of acid phosphatase at pH3 and pH5
