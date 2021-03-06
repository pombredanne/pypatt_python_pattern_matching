# PyPatt

Python Pattern Matching.

```python
def test_demo():
    values = [[1, 2, 3], ('a', 'b', 'c'), 'hello world',
        False, [4, 5, 6], (1, ['a', True, (0, )], 3)]

    for value in values:
        match value:
            like 'hello world':
                print 'Match strings!'
            like False:
                print 'Match booleans!'
            like [1, 2, 3]:
                print 'Match lists!'
            like ('a', 'b', 'c'):
                print 'Match tuples!'
            like [4, 5, temp]:
                print 'Bind variables! temp =', temp
            like (1, ['a', True, result], 3):
                print 'Supports nesting! result =', result
```

Introduces two new keywords to the Python programming language:

* match_stmt = match <expr>:
* like_stmt = like <expr>:

Limitations abound. But it's still useful and really cool as a
proof-of-concept project.

* See https://github.com/dropbox/pyxl for the codec inspiration.

# Notes

* Make sure "import pypatt" is in your site.py file. (This will require that
  pypatt be on your pythonpath.)
* Make sure "# coding: pypatt" is the first line in your Python file.
* Only 'True'/'False' match booleans (no type coercion).
* '_' matches anything even if already defined in locals or globals and binds
  to the last matched item. For example:

```python
    _ = 'test'
    match value:
        like [_, _]:
            print 'match'
```

  will match all lists of length 2 and '_' will be bound to the
  item in value[1].
* Beware of matching variables in a loop like:

```python
    for value in xrange(5):
        match value:
            like 1:
                print 'saw 1'
            like value:
                print 'saw', value
                # Be sure to delete the bound variable otherwise,
                # the first iteration will bind 'value' to 0 and
                # the third iteration will not match 'value'.
                del value
```

* 'exec' is used to insert bound variables into the local scope. In Python
  2.* 'exec' may not be used with nested functions.

# Testing

* Requires 'nose'

## Passing

* Python 2.7 (r27:82525, Jul  4 2010, 09:01:59) [MSC v.1500 32 bit (Intel)] on win32

# TODO

* Add "as" syntax:

```python
    match list(generator) as items:
        like [1, 2, 3, 4]:
            print items
```

* Use Ellipsis to mean ... match anything in the rest of this list or tuple.

```python
    match [1, 2, 3, 4]:
        like [1, 2, Ellipsis]:
            print 'Ellipsis matched 3, 4.'
```

* Support value as iterable?

```python
    match iter([1, 2, 3, 4]):
        like [1, 2, Ellipsis, tail]:
            return tail # This is the iterator pointing at 3
```

* Emacs mode based on Python which recognizes the 'match' and 'like' keywords.
* Match dict expression (require bound keys, allow Ellipsis)
* Support Attribute lookup e.g. "TypeName.Attr"
* Support function calls
* Add "when" syntax like:

```python
    match list_item:
        like [first, second] when first < second:
            print 'ascending'
        like [first, second] when first > second:
            print 'descending'
```

## Add Codec Tools

* As a module:

```python
python -m codectools --codec pypatt --exec program.py
```

Includes pypatt codec before exec'ing program.py. So that the codec doesn't have to be put in the site.py file during development.

* As a codec:

```python
# coding: codectools
# usecodecs: pyxl pypatt
```

This will run the code first through the pyxl codec and then through the pypatt codec.
