
 

 

 

 

 

([C++](Cpp.md)) [\_algo.c: Call to undefined function 'rand'](CppCompileError_algoCcallToUndefinedFunctionRand.md)
====================================================================================================================

 

[Compile error](CppCompileError.md).

 

 

 

 

Full error message
------------------

 

  ----------------------------------------------------------------------
  ` [C++ Error] _algo.c(432): E2268 Call to undefined function 'rand'`
  ----------------------------------------------------------------------

 

This takes you to the following code in \_algo.c:

 

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  ` // Return a random number in the range [0, __n).  This function encapsulates // whether we're using rand (part of the standard C library) or lrand48 // (not standard, but a much better choice whenever it's available).  template <class _Distance> inline _Distance __random_number(_Distance __n) { #ifdef _STLP_NO_DRAND48   return rand() % __n; //THIS LINE #else   return lrand48() % __n; #endif }`
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

 

 

 

Cause
-----

 

[IDE](CppIde.md): [C++ Builder](CppBuilder.md) 6.0

[Compiler](CppCompiler.md): Borland BCC32.EXE version 6.0.10.157

Project type: Console Application

 

The [compiler](CppCompiler.md) cannot find the function rand (note that
it does not complain that it cannot find [std::rand](CppStdRand.md)). But
it will only need rand when making use of the function above. For
example, when you use [std::random\_shuffle](CppStdRandom_shuffle.md).

 

  ---------------------------------------------------------------------------------------------------------------------------
  ` #include <algorithm> #include <vector>  int main() {   std::vector<int> v;   std::random_shuffle(v.begin(),v.end()); }`
  ---------------------------------------------------------------------------------------------------------------------------

 

 

 

 

 

Solution
--------

 

It is not a good idea to modify standard [header
files](CppHeaderFile.md). But the [compiler](CppCompiler.md) cannot
find rand, but it can find [std::rand](CppStdRand.md). So I add the
[namespace](CppNamespace.md) [std](CppStd.md) to rand. And I have
never had trouble with this.

 

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  ` // Return a random number in the range [0, __n).  This function encapsulates // whether we're using rand (part of the standard C library) or lrand48 // (not standard, but a much better choice whenever it's available).   template <class _Distance> inline _Distance __random_number(_Distance __n) { #ifdef _STLP_NO_DRAND48   return std::rand() % __n; //THIS LINE #else   return lrand48() % __n; #endif }`
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

 

 

 

 

 

