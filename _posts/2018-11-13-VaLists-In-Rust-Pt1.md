---
layout:   post
type:     blog
title:    VaLists in Rust - pt. 1
date:     2018-11-13
summary:  Using the strange and terrifying va_list in Rust
keywords: va_list, varargs, c, rust
image:    dot-dot-dot.png
---

## Overview

After [rust-lang/rust#49878] (The first PR implementing [RFC 2137])
*some* support for `va_list`s has been merged into base [rust].
Variadic arguments are used by C functions when a variable number
of arguments need to be passed to an argument, e.g. `printf()`.

As an example, we can create a function `add_n()` that will add up
to `n` integers passed to the function. A simple implementation of
this function in C might look something like the following:

```c
#include <stdarg.h>

int add_n(int n, ...) {
  va_list ap;
  int i, sum = 0;
  va_start(ap, n);
  for (i = 0; i < n; ++i) {
    sum += va_arg(ap, int);
  }
  va_end(ap);
  return sum;
}
```

The important pieces of this function are the following:

 - The `va_start` macro initializes the `va_list`
 - The `va_arg` macro fetches the next value of
   the given type from the `va_list`.
 - The `va_end` macro is used after we're done
   with the list.

We can now use `add_n()` with something like the following:

```c
  assert(add_n(3, 1, 2, 3) == 6);
  assert(add_n(2, 20, 22) == 42);
```

## `vprintf()`-like functions

Most procedures that use `va_list`s are actually broken across two
functions. For example, `printf()` is accompanied by `vprintf()`.
Let's do the same with our `add_n()` function, by adding a `vadd_n()`
function.

```c
#include <stdarg.h>

int vadd_n(int n, va_list ap) {
  int i, sum = 0;
  for (i = 0; i < n; ++i) {
    sum += va_arg(ap, int);
  }
  return sum;
}

int add_n(int n, ...) {
  va_list ap;
  int sum;
  va_start(ap, n);
  sum = vadd_n(n, ap);
  va_end(ap);
  return sum;
}
```

Note that all of the initialization and management of the `va_list`
occurs in `add_n()`, while the "real" work occurs in `vadd_n()`.

## Writing `vprintf()`-like functions in rust

We can implement `vadd_n()` in [rust] with something like the following:

```rust
#![feature(c_variadic)]

use libc::c_int;
use std::ffi::VaList;

#[no_mangle]
pub unsafe fn vadd_n(argc: c_int, ap: VaList) -> c_int {
    let mut sum = 0;
    for i in 0..argc {
        sum += ap.arg::<c_int>();
    }
    sum
}
```

`VaList` is experimental and only available in `nightly` [rust] with the `c_variadic`
feature enabled.

[rust-lang/rust#49878] is the first of many PRs to add support for `va_list`s in rust.
It only allows for implementing `vprintf()`-like functions. Support for implementing
functions that require initializing the `va_list` will be added in a follow-up
PR, so for now we'll have to implement `add_n()` in C with something like the following:

```c
#include <stdarg.h>

extern int vadd_n(int, va_list);

int add_n(int argc, ...) {
  va_list ap;
  int sum;
  va_start(ap, argc);
  sum = vadd_n(int, va_list);
  va_end(ap);
  return sum;
}
```

Huzzah!

[RFC 2137]: https://github.com/rust-lang/rfcs/pull/2137
[rust]: https://rust-lang.org
[rust-lang/rust#49878]: https://github.com/rust-lang/rust/pull/49878
