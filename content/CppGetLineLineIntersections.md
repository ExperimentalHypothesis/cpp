
 

 

 

 

 

([C++](Cpp.md)) [GetLineLineIntersections](CppGetLineLineIntersections.md)
============================================================================

 

[GetLineLineIntersections](CppGetLineLineIntersections.md) is a [code
snippet](CppCodeSnippets.md) to calculate the zero or one intersecion
points of two lines.

 

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  ` #include <boost/geometry.hpp> #include <boost/geometry/geometries/linestring.hpp> #include <boost/geometry/geometries/point_xy.hpp>  ///Obtain the zero or one intersections between two finite lines //From http://www.richelbilderbeek.nl/CppGetLineLineIntersections.htm template <class T> const std::vector<   boost::geometry::model::d2::point_xy<T> > GetLineLineIntersections(   const boost::geometry::model::linestring<     boost::geometry::model::d2::point_xy<T>   > line1,   const boost::geometry::model::linestring<     boost::geometry::model::d2::point_xy<T>   > line2) {   typedef boost::geometry::model::d2::point_xy<T> Point;   typedef boost::geometry::model::linestring<Point> Line;   std::vector<Point> points;   boost::geometry::intersection(line1,line2,points);   assert(points.empty() || points.size() == 1);   return points; }`
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

 

 

 

 

Testing code
------------

 

See [Boost.Geometry example 2: Calculate the intersection of two
lines](CppGeometryExample2.md) for the testing code.

 

 

 

 

 

 

