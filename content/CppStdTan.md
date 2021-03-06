
 

 

 

 

 

([C++](Cpp.md)) [std::tan](CppStdTan.md)
=======================================

 

[std::tan](CppStdTan.md) (an abbreviation of 'tangens') is a
[mathematical](CppMath.md) [STL](CppStl.md)
[function](CppFunction.md) that [returns](CppReturn.md) the tangens of
an inputted value. This value must be supplied in radians.

 

  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  ` #include <cmath> #include <iostream>  int main() {   std::cout << std::showpos;   for(int i = -8; i != 9; ++i)   {     const double x = static_cast<double>(i) * (0.125 * M_PI);     const double y = std::tan(x);     std::cout << "std::tan(" << i << "/8 pi) = " << y << '\n';   } }`
  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Screen output:

 

  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  ` std::tan(-8/8 pi) = +1.22461e-16 std::tan(-7/8 pi) = +0.414214 std::tan(-6/8 pi) = +1 std::tan(-5/8 pi) = +2.41421 std::tan(-4/8 pi) = -1.63318e+16 std::tan(-3/8 pi) = -2.41421 std::tan(-2/8 pi) = -1 std::tan(-1/8 pi) = -0.414214 std::tan(+0/8 pi) = +0 std::tan(+1/8 pi) = +0.414214 std::tan(+2/8 pi) = +1 std::tan(+3/8 pi) = +2.41421 std::tan(+4/8 pi) = +1.63318e+16 std::tan(+5/8 pi) = -2.41421 std::tan(+6/8 pi) = -1 std::tan(+7/8 pi) = -0.414214 std::tan(+8/8 pi) = -1.22461e-16`
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

 

 

 

 

 

