---
title: Projects with Multiple Targets
keywords: class
last_updated: Apr 4, 2020
tags: [tools modern]
summary: ""
permalink: mydoc_modern_complex.html
folder: mydoc
---

Objective-C is an [Object Oriented Programming Language](https://en.wikipedia.org/wiki/Object-oriented_programming).
With that comes the expectation of a [plug-n-play](https://dl.acm.org/doi/10.1145/2601328.2601334) programming environment.

It should be possible to add and remove functionality, without
breaking the application. This expectation has been historically never
fulfilled, due to deficiencies with the compilation tools, the Objective-C
runtime and the way headers are handled.

A golden rule of mulle-objc is: do not create projects
with multiple targets. Instead create multiple projects and link them
together via dependencies.


## Setting up a complex project for the modern workflow

If you have a library and an executable, make them two
separate projects.

``` console
mulle-sde init -m foundation/objc-developer -d mylib library
mulle-sde init -m foundation/objc-developer -d myexe executable
```

Your project layout would then look like this:

```
myproject
├── myexe
└── mylib
```

As you want to use the library with your executable, you add the library as a
dependency to the executable:

``` console
cd myexe
mulle-sde dependency add --objc --github "${GITHUB_USERNAME:-nobody}" mylib
```

The `GITHUB_USERNAME` can be fake, but it is needed to create a URL
for the library project, just in case you want to distribute your executable
later. It can also be changed later.


{% include tip.html content="Since `mylib` is a sibling of `myexe` it
will be easily found. If you place it somewhere else, you need to add it's
parent directory to the search-path variable `MULLE_FETCH_SEARCH_PATH`.
E.g. `mylib` is `/home/wherever/src/mylib` then modify the search-path with `mulle-sde environment set MULLE_FETCH_SEARCH_PATH '/home/wherever/src:${MULLE_FETCH_SEARCH_PATH}'`" %}

With `mulle-sde dependency list` or the more low-level `mulle-sourcetree list -ll`
you can see the dependency structure of your project.

```
address             marks                                           aliases  include
-------             -----                                           -------  -------
Foundation          no-singlephase
Foundation-startup  no-dynamic-link,no-header,no-intermediate-link
mylib               no-singlephase
```

At this point you're ready to craft again.

``` console
mulle-sde craft
```

And there you are.

{% include note.html content="You do not have to `#import` anything or add link
statements to your `CMakeLists.txt`, it's all done for you with
`mulle-sde reflect`." %}


## Next

Alright lets actually look at the [language](https://mulle-objc.github.io/De-Re-mulle-objc/mydoc_good.html) as it's implemented by mulle-objc.