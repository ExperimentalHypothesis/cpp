
 

 

 

 

 

([C++](Cpp.md)) [std::setiosflags](CppSetiosflags.md)
=======================================================

 

[std::setiosflags](CppSetiosflags.md) (an abbreviation of 'set
input/output [stream](CppStream.md) flags') is a
[stream](CppStream.md) modifier.

 

  ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  ` #include <iomanip> #include <iostream>  int main() {   const double x = 12.34;   std::cout     << std::setiosflags(std::ios::scientific)     << x << '\n'     << std::setiosflags(std::ios::fixed)     << x << '\n'; }`
  ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Screen output:

 

  -----------------------
  ` 1.234000e+01 12.34`
  -----------------------

 

 

 

 

 

 

