---
layout: nil
---

Mike Morearty's code
====================

Hardware breakpoints
--------------------

A C++ class to allow you to very easily set hardware breakpoints from within
your program.  This can be used, for example, if a particular variable is
getting trashed.

Type-safe, buffer-safe printf to a C++ ostream or string
--------------------------------------------------------

    void oprintf( std::ostream&, printf_format, ... );
    std::string strprintf( printf_format, ... );
    std::wstring wstrprintf( printf_format, ... );

These functions provide type-safe printf!  For example:

    oprintf( cout, "%s", 3 ); // run-time assertion: type mismatch
    oprintf( cout, "%s" ); // run-time assertion: too few arguments
    oprintf( cout, "hello", 3 ); // run-time assertion: too many arguments

Also, they provide buffer-safe printf, since they write to an ostream or a C++
string.  Finally, as with the previous, MFC-based code sample, these functions
can be very handy for calling a function without having to explicitly create a
string temporary:

    MessageBox( hwnd, strprintf( "error %d", errorcode ).c_str(), NULL, MB_OK );

Printf directly to an MFC CString
---------------------------------

This is a function which makes it easy to elegantly create a temporary `char*` or
`CString` and pass it as an argument, without having to explicitly create a
temporary variable. For example, you can combine these three lines...

    CString s;
    s.Format(format_string, args ...);
    AfxMessageBox(s);

... into this one line:

    AfxMessageBox( StrPrintf(format_string, args ...) );

IDispatch wrapper with Get("property"), Put/PutRef("property", value), Invoke("method", args)
---------------------------------------------------------------------------------------------

ATL, MFC, and the Visual C++ runtime library already have wrappers for
IDispatch (CComDispatchDriver, COleDispatchDriver, and the `_com_dispatch_...`
methods, respectively). However, all three of these suffer from syntax that is
much more awkward than what you can write in languages such as Visual Basic and
Javascript.  This wrapper lets you write code like this:

    _bstr_t html = htmldoc.Get("body").Get("innerHTML");
    htmldoc.Put("title", "New Title");
    htmldoc.Get("body").Get("firstChild").Invoke(
        "insertAdjacentText", "afterBegin", "hello world");
