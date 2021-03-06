
 

 

 

 

 

([C++](Cpp.md)) [TDD](CppTdd.md)
==================================

 

'[Test Driven Development (TDD)](CppTdd.md) is a software design
technique' \[1\].

 

Here is an example of a set of rules to work following [TDD](CppTdd.md)
\[2\](citing \[3\]):

 

1.  Write production code only to pass a failing unit test
2.  Write no more of a unit test than sufficient to fail (compilation
    failures are failures)
3.  Write no more production code than necessary to pass the one failing
    unit test

 

 

 

 

Notes to self
-------------

 

-   Classes used in a [desktop application](CppDesktopApplication.md)
    take much longer to write tests for. Consider writing 'TestMyClass'
    applications to see the class in isolation. If visual class has
    non-visual data members, these non-visual data members must be
    tested for emitting signals (notifiying these visual class
    to update) and being in sync with the visual class. If you refrain
    from doing this, you will regret it eventually and write these
    tests later.

 

 

 

 

 

[References](CppReferences.md)
-------------------------------

 

1.  [Jeff Langr](CppJeffLangr.md). Modern C++ Programming with
    Test-Driven Development. 2013. ISBN: 978-1-937785-48-2. Introduction
    chapter, page xiv: 'Test-Driven Development (TDD), a software design
    technique devised in the late 1990s, \[...\]'
2.  [Jeff Langr](CppJeffLangr.md). Modern C++ Programming with
    Test-Driven Development. 2013. ISBN: 978-1-937785-48-2. Chapter 3.4,
    page 59.
3.  Robert C. Martin ('Uncle Bob').
    http://butunclebob.com/ArticleS.UncleBob.TheThreeRulesOfTdd

 

 

 

 

 

 

