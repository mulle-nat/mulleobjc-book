---
title: Printing
keywords: class
last_updated: February 26, 2022
tags: [language]
summary: "Printing functions"
permalink: mydoc_printing.html
folder: mydoc
---

## Intro

The C way to print anything to a file, the console or even to memory
is `printf` and friend functions which are defined in `<stdio.h>`. `printf` is still absolutely viable with mulle-objc. To print an object with standard
printf, you would use the objects `-UTF8String` method:

``` c
printf( "%s\n", [obj UTF8String]);
```

But when objects are involved it is more convenient to use the
`mulle_printf` family of functions. `mulle_printf` allows the special '%@'
object conversion. This simplifies the code to:

``` c
mulle_printf( "%@\n", obj);
```

Technically the same thing happens as in the printf example: the `-UTF8String` method of **obj** is called and the result is used to replace `%@`.


{% include note.html content="In many Objective-C tutorials you will
see <tt>NSLog</tt> being used instead of printf. This is not a good
replacement though, as <tt>NSLog</tt> actually also writes into the system log." %}

## Functions with C format strings

### Functions with normal arguments

| stdio       | mulle              | Description
|-------------|--------------------|------------------
| `printf`    | `mulle_printf`     | print to stdout
| `fprintf`   | `mulle_fprintf`    | print to FILE
| `dprintf`   | -                  | print to file descriptor
| `sprintf`   | `mulle_sprintf`    | print to "big enough" char buffer
| `snprintf`  | `mulle_snprintf`   | print to char buffer of given size

There are some non-POSIX extensions which are available on BSD and Linux for C.
The mulle functions are available on all platforms.

| stdio       | mulle              | Description
|-------------|--------------------|------------------
| `asprintf`  | `mulle_asprintf`   | allocate char buffer and print to char buffer of given size with varargs (C)
| `vasprintf` | `mulle_vasprintf`  | allocate char buffer and print to char buffer of given size with varargs (C)


### Functions with C varargs

The `vprintf` variants accept stdarg parameters. This is convenient for
forwarding arguments in a C function, that itself accepts variable arguments.

| stdio       | mulle              | Description
|-------------|--------------------|------------------
| `vprintf`   | `mulle_vprintf`    | print to stdout with stdarg (C)
| `vfprintf`  | `mulle_vfprintf`   | print to FILE with stdarg (C)
| `vdprintf`  | -                  | print to file descriptor with stdarg (C)
| `vsprintf`  | `mulle_vsprintf`   | print to "big enough" char buffer with stdarg (C)
| `vsnprintf` | `mulle_vsnprintf`  | print to char buffer of given size with stdarg (C)


### Functions with mulle-varargs

Methods with variable arguments don't receive stdarg arguments but
mulle-varargs arguments. There is support for this as well.

| Function           | Description
|--------------------|------------------
| `mulle_mvprintf`   | print to stdout with mulle-varargs (Objective-C)
| `mulle_mvfprintf`  | print to FILE with mulle-varargs (Objective-C)
| `mulle_mvsprintf`  | print to "big enough" char buffer with mulle-varargs (Objective-C)
| `mulle_mvsnprintf` | print to char buffer of given size with mulle-varargs (Objective-C)
| `mulle_mvasprintf` | allocate char buffer and print with mulle-varargs (Objective-C)


### Functions printing into autoreleased C strings

With `asprintf` a `malloc`ed string is returned. The caller should
`free` this string eventually. Wouldn't it be convenient, if the string was
already autoreleased, so the caller wouldn't have to free it ?

You can create such autoreleases C strings like so:

```c
char   *s;

s = MulleObjC_asprintf( "%d", 1848);
```

The following mulle functions create autoreleased C strings:


| Function               | Description
|------------------------|------------------
| `MulleObjC_asprintf`   | allocate autoreleased char buffer and print to it
| `MulleObjC_vasprintf`  | allocate autoreleased char buffer and print to it with varargs (C)
| `MulleObjC_mvasprintf` | allocate autoreleased char buffer and print to it with mulle-varargs (Objective-C)


## Functions with Objective-C format strings

`MulleObjCPrintf( @"%@\n", @VfL Bochum 1848")` is basically the same as
`mulle_printf( ["@%s\n" UTF8String], @"VfL Bochum 1848\n")`.

MulleObjC supports Objective-C format strings with the following functions:

| C                 | Objective-C          | Description
|-------------------|----------------------|------------------
| `mulle_printf`    | `MulleObjCPrintf`    | print to stdout
| `mulle_vprintf`   | `MulleObjCVPrintf`   | print to stdout with stdarg (C)
| `mulle_mvprintf`  | `MulleObjCVPrintf`   | print to stdout with mulle-vararg (Objective-C)
| `mulle_fprintf`   | `MulleObjCFPrintf`   | print to FILE
| `mulle_vfprintf`  | `MulleObjCVPrintf`   | print to FILE with stdarg (C)
| `mulle_mvfprintf` | `MulleObjCMVPrintf`  | print to FILE with mulle-vararg (Objective-C)

For all other purposes use the `NSString stringWithFormat:` methods.


## Next

Ok lets have some fun with writing [Plug and Play Classes](mydoc_pnp_class.html)

