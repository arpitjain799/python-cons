[![Build Status](https://travis-ci.org/pythological/python-cons.svg?branch=master)](https://travis-ci.org/pythological/python-cons) [![Coverage Status](https://coveralls.io/repos/github/pythological/python-cons/badge.svg?branch=master)](https://coveralls.io/github/pythological/python-cons?branch=master) [![PyPI](https://img.shields.io/pypi/v/cons)](https://pypi.org/project/cons/)

Python `cons`
==================

An implementation of [`cons`][cons] in Python.

Usage and Design
======================

The `cons` package attempts to emulate the semantics of Lisp/Scheme's `cons` as closely as possible while incorporating all the built-in Python sequence types:
```python
>>> from cons import cons, car, cdr
>>> cons(1, [])
[1]

>>> cons(1, ())
(1,)

>>> cons(1, [2, 3])
[1, 2, 3]
```

In general, `cons` is designed to work with `collections.abc.Sequence` types.

According to the `cons` package, `None` corresponds to the empty built-in `list`, as `nil` does in some Lisps:
```python
>>> cons(1, None)
[1]
```

The `cons` package follows Scheme-like semantics for empty sequences:
```python
>>> car([])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ConsError: Not a cons pair

>>> cdr([])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ConsError: Not a cons pair

```

By default, `str` types are not considered cons-pairs, although they are sequences:
```python
>>> cons("a", "string")
ConsPair('a' 'a string')
```

This setting can be overridden and other types can be similarly excluded from consideration by registering classes with the `abc`-based classes `MaybeCons` and `NonCons`.


Features
===========

* Built-in support for the standard Python ordered sequence types: i.e. `list`, `tuple`, `Iterator`, `OrderedDict`.
```python
>>> from collections import OrderedDict
>>> cons(('a', 1), OrderedDict())
OrderedDict([('a', 1)])

```
* Existing `cons` behavior can be changed and support for new collections can be added through the generic functions `cons.core._car` and `cons.core._cdr`.
* Built-in support for [`unification`][un].
```python
>>> from unification import unify, reify, var
>>> unify([1, 2], cons(var('car'), var('cdr')), {})
{~car: 1, ~cdr: [2]}

>>> reify(cons(1, var('cdr')), {var('cdr'): [2, 3]})
[1, 2, 3]

>>> reify(cons(1, var('cdr')), {var('cdr'): None})
[1]

```

Installation
================

```python
pip install cons
```


[cons]: https://en.wikipedia.org/wiki/Cons
[un]: https://github.com/mrocklin/unification
