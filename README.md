# FixedNumbers.jl

[![Build Status](https://travis-ci.org/perrutquist/FixedNumbers.jl.svg?branch=master)](https://travis-ci.org/perrutquist/FixedNumbers.jl)
[![codecov.io](http://codecov.io/github/perrutquist/FixedNumbers.jl/coverage.svg?branch=master)](http://codecov.io/github/perrutquist/FixedNumbers.jl?branch=master)

This package provides `Fixed` `Number` datatypes which store their data in a
type parameter.

Data that is passed in type parameters is (usually) handled at compile-time,
rather than at run-time. In certain cases this can lead to
better performance.
For this reason, some functions accept "value type" arguments, `Val{X}()`,
where `X` is an argument that is passed at compile-time. (See the Julia
documentation.) `Fixed` is an alternative to `Val` which is specifically
designed to handle numbers.

The difference between `Val` and `Fixed` is that `Fixed` types
promote and convert like their type parameters, so they can be used directly in
arithmetic operations. (For example `Fixed(1) + 1` equals `2`.)
This makes it possible to use them with functions that were not specifically
written to accept value arguments.

Under the surface, there are three `Fixed` datatypes: `FixedInteger`,
`FixedReal`, and `FixedNumber`, subtypes of `Integer`, `Real` and `Number`
respectively. The `Union` type `Fixed` can be used to refer to them all.
For brevity, all three types are displayed as `Fixed(X)`, and it is also
recommended to create them using this syntax.

By default, any operation on a `Fixed` will result in a non-`Fixed` type.
For example, `Fixed(1)*Fixed(1)` gives `1` -- not `Fixed(1)`.
It is possible to change this behavior by method overloading.
The `@fixednumbers` macro can be used on a small set of `Fixed` numbers
to make certain operations preserve the `Fixed` type when possible.

There is no `FixedRational` datatype, but a `FixedReal` with a
`Rational` type parameter will convert and promote like its parameter.
For example: `Fixed(1//2) + 1 === 3//2`.

Note: `Fixed` numbers are only fast when fully specified. A `Vector{Fixed}`
is much slower than a `Vector{Int}`.
(A `Vector{FixedInteger{1}}` is fast and requires very little memory,
but on the other hand it can only store the number one.)