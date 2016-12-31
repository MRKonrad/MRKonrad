---
layout: post
title: "How to configure CMake, googleTest, XCode and an external library to work together"
date:   2016-12-11
categories: CMake, googleTest, XCode, VXL
tags: Tutorial
---

In this post I give a list of useful tips to make CMake, googleTest, XCode and an external library work together.

## Make install

When you compile a library, like VXL or ITK with unix makefiles, you can use ``make install`` after ``make``. This way you can use ``find_package(<package_name>)``.

## List all variables after find_package
When writing your own and you use CMakeLists.txt and you use ``find_package(<package_name>)``, it might be useful to list all variables. It makes much easier to find library and include paths variables.

{% highlight cmake%}
get_cmake_property(_variableNames VARIABLES)
foreach (_variableName ${_variableNames})
  message(STATUS "${_variableName}=${${_variableName}}")
endforeach() #_)
{% endhighlight %}

## Other
Variables listing all files:
{% highlight cmake%}
file(GLOB_RECURSE sources_code code/*.cc code/*.cpp code/*.h)
file(GLOB_RECURSE sources_test test/*.cc code/*.cpp test/*.h)
{% endhighlight %}

Printing variables:
{% highlight cmake %}
MESSAGE( STATUS "sources_code:         " ${sources_code} )
MESSAGE( STATUS "sources_test:         " ${sources_test} )
{% endhighlight %}

Adding executables. Remember that in each executable you can have just one ``main()``.
{% highlight cmake %}
add_executable (myproject_code ${sources_code})
add_executable (myproject_test ${sources_test})
{% endhighlight %}

Overwriting non-intuitive division of code into source groups visible in an IDE, like XCode
{% highlight cmake %}
source_group("" FILES ${sources_code})
source_group("" FILES ${sources_test})
{% endhighlight %}

## Useful links

[Cmake](https://cmake.org)<br/>
[Cmake tutorial](https://cmake.org/cmake-tutorial/)<br/>
[GoogleTest](https://github.com/google/googletest)<br/>
[VXL](https://github.com/vxl/vxl)<br/>
