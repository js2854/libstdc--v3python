# libstdc--v3python

## STL Support Tools(https://sourceware.org/gdb/wiki/STLSupport)

When you try to use GDB's "print" command to display the contents of a vector, a stack, or any other GDB abstract data structure, you will get useless results. Instead, download and install one of following tools to properly view the contents of STL containers from within GDB.

 * [[GDB_7.0_Release|GDB 7.0]] will include support for writing pretty-printers in Python.  This feature, combined with the pretty-printers in the libstdc++ svn repository, yields the best way to visualize C++ containers.  Some distros (Fedora 11+) ship all this code in a way that requires no configuration; in other cases, [[http://lists.kde.org/?l=kdevelop&m=125326438617051&w=2|this email message]] explains how to set everything up. The main points have been redacted here:
  1. Check-out the latest Python libstdc++ printers to a place on your machine.  In a local directory, do:
  
    svn co svn://gcc.gnu.org/svn/gcc/trunk/libstdc++-v3/python

  1. Add the following to your ~/.gdbinit.  The path needs to match where the python module above was checked-out.  So if checked out to: /home/maude/gdb_printers/, the path would be as written in the example:

    python
    import sys
    sys.path.insert(0, '/home/maude/gdb_printers/python')
    from libstdcxx.v6.printers import register_libstdcxx_printers
    register_libstdcxx_printers (None)
    end

 The path should be the only element that needs to be adjusted in the example above. Once loaded,  STL classes that the printers support should printed in a more human-readable format.  To print the classes in the old style, use the {{{/r}}} (raw) switch in the print command (i.e., {{{print /r foo}}}). This will print the classes as if the Python pretty-printers were not loaded.

 * '''gdb-stl-views''' is a set of GDB macros that can display the contents of many STL containers: list, vector, map, multimap, set, multiset, dequeue, stack, queue, priority_queue, bitset, string, and widestring. Writen and currently maintained by Dan Marinescu - PhD. The author formally disclaims copyright to this source code.  In place of a legal notice, here is a blessing: May you do good and not evil. May you find forgiveness for yourself and forgive others. May you share freely, never taking more than you give!
  * You can download it [[attachment:stl-views-1.0.3.gdb|here]] or http://www.yolinux.com/TUTORIALS/src/dbinit_stl_views-1.03.txt

  * Tutorials and an alternative download are [[http://www.yolinux.com/TUTORIALS/GDB-Commands.html#STLDEREF|hosted at yolinux.com]].

 * '''gdb++''' is a Perl script which extends gdb. It comes bundled as part of the Devel::GDB::Reflect Perl module. First [[http://perl.about.com/od/packagesmodules/qt/perlcpan.htm|use CPAN]] to install the module, then follow the [[http://search.cpan.org/perldoc?gdb++#DESCRIPTION|gdb++ usage instructions]]. Developed by Stanford PhD student Antal Novak.

 * There are other options. Tom Malnar wrote a set of GDB macros similar to Dan's ( http://thread.gmane.org/gmane.comp.gcc.g++.general/4060/focus=4167 ) but it doesn't cover as wide a variety of containers. Gilad Mishne wrote a different set of macros ( http://www.stanford.edu/~afn/gdb_stl_utils/ ) but it is long unmaintained and it works only with SGI's STL implementation, which very few GCC users use.

 * Iterators: how to display the item the iterator points at (tested on gdb 6 with a list): ''print *(iter._M_current)''
