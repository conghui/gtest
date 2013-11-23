# Gtest

This repo contains the headers and compiled static library (64 bits) of 
[Google C++ Testing Framework](https://code.google.com/p/googletest/).

It is commonly used as a submodules included in a project as a testing 
framework. 

# How to include the repo in your GNU Autotools project

### Copy the headers to the current project

In your project, you can clone this repo 

{% highlight bash %}

    git clone git@github.com:conghui/gtest.git
{% endhighlight %}


### Write a gtest program 

I create a `test` folder in the project's root folder, and now I create a 
gtest program, say `gtest.cxx`, in the `test` folder.

{% highlight cpp %}

    #include "gtest/gtest.h" // we will add the path to C preprocessor later

    int main(int argc, char **argv)
    {
      ::testing::InitGoogleTest(&argc, argv);

      return RUN_ALL_TESTS();
    }

{% endhighlight %}

Notice that, the header `gtest/gtest.h` is not at the `test` folder. 
Instead, it is at `my-project-path/gtest/include`. The reason that we can 
include `gtest/gtest.h` directly in the file is that we will add 
`my-project-path/gtest/include` to the path of the C preprocessor.

### Write Makefile.am

In the Makefile.am, we need to specific the following things:
1. the name of the program to be tested (run)
2. the name of the program to be compiled (not run)
3. the other flags

See the content of Makefile.am

{% highlight makefile %}

    LDADD =  ../gtest/lib/libgtest.a

    AM_CPPFLAGS = -I../gtest/include

    LIBS += -lpthread

    TESTS = gtest

    check_PROGRAMS = gtest

    AM_DEFAULT_SOURCE_EXT = .cxx

{% endhighlight %}

`LDADD` specific the path of gtest library and `AM_CPPFLAGS` specific the 
include path of gtest.

### Run
Now, you can run `make check` to pass the test.

# Reference
1. [Integrate gtest (google test framework) into GNU Autotools 
   Project](http://conghui.github.io/2013/11/20/integrate-gtest-into-autotools/)
2. [Google C++ Testing Framework](https://code.google.com/p/googletest/)
