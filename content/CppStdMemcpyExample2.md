
 

 

 

 

 

([C++](Cpp.md)) [StdMemcpyExample2](CppStdMemcpyExample2.md)
==============================================================

 

Technical facts
---------------

 

[Operating system(s) or programming environment(s)](CppOs.md)

-   ![Lubuntu](PicLubuntu.png) [Lubuntu](CppLubuntu.md) 15.04 (vivid)

[IDE(s)](CppIde.md):

-   ![Qt Creator](PicQtCreator.png) [Qt Creator](CppQtCreator.md) 3.1.1

[Project type](CppQtProjectType.md):

-   ![console](PicConsole.png) [Console
    application](CppConsoleApplication.md)

[C++ standard](CppStandard.md):

-   ![C++98](PicCpp98.png) [C++98](Cpp98.md)

[Compiler(s)](CppCompiler.md):

-   [G++](CppGpp.md) 4.9.2

[Libraries](CppLibrary.md) used:

-   ![STL](PicStl.png) [STL](CppStl.md): GNU ISO C++ Library, version
    4.9.2

 

 

 

 

 

[Qt project file](CppQtProjectFile.md): ./CppStdMemcpyExample2/CppStdMemcpyExample2.pro
----------------------------------------------------------------------------------------

 

  ---------------------------------------------------------------
  ` include(../../ConsoleApplication.pri)  SOURCES += main.cpp`
  ---------------------------------------------------------------

 

 

 

 

 

./CppStdMemcpyExample2/main.cpp
-------------------------------

 

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  ` #include <cassert> #include <iostream> #include <cstring> #include <chrono> #include <vector>   template<class T> void CopyStl(const std::vector<T>& from, std::vector<T>& to) {   assert(from.size() == to.size());   std::copy(std::begin(from),std::end(from),std::begin(to)); }  template<class T> void CopyMemcpy(const std::vector<T>& from, std::vector<T>& to) {   assert(from.size() == to.size());   assert(from.size() * sizeof(int) > 0);   std::memcpy(&to[0],&from[0],from.size() * sizeof(int)); }  int main() {   using Clock = std::chrono::high_resolution_clock;   using Duration = std::chrono::system_clock::duration;   using TimePoint = std::chrono::high_resolution_clock::time_point;   using Msecs = std::chrono::milliseconds;    const int sz{100000000};   const int n{100};   const std::vector<int> from(sz);   std::vector<int> to(from.size());    const TimePoint t_stl_begin{Clock::now()};    for (int i=0; i!=n; ++i) { CopyStl(from,to); }    const Duration t_stl_time{Clock::now() - t_stl_begin};   const TimePoint t_memcpy_begin{Clock::now()};    for (int i=0; i!=n; ++i) { CopyMemcpy(from,to); }    const Duration t_memcpy_time{Clock::now() - t_memcpy_begin};    #ifndef NDEBUG   std::cout << "Don't do profing in debug mode!" << std::endl;   #else   std::cout     << std::chrono::duration_cast<Msecs>(t_stl_time).count() << " milliseconds" << '\n'     << std::chrono::duration_cast<Msecs>(t_memcpy_time).count() << " milliseconds" << '\n'     << std::endl;   #endif }  /* Screen output:  9222 milliseconds 9140 milliseconds  Press <RETURN> to close this window...  */`
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

 

 

 

 

 

