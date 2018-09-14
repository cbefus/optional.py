# Optional.py
[![Build Status](https://img.shields.io/pypi/v/optional.py.svg)](https://pypi.org/project/optional.py/)
[![Build Status](https://travis-ci.com/cbefus/optional.py.svg?branch=master)](https://travis-ci.com/cbefus/optional.py)
[![Coverage Status](https://coveralls.io/repos/github/cbefus/optional.py/badge.svg?branch=master)](https://coveralls.io/github/cbefus/optional.py?branch=master)
[![License](https://img.shields.io/pypi/l/optional.py.svg)](https://pypi.org/project/optional.py/)
[![Python Versions](https://img.shields.io/pypi/pyversions/optional.py.svg)](https://pypi.org/project/optional.py/)
[![Contributors](https://img.shields.io/github/contributors/cbefus/optional.py.svg)](https://pypi.org/project/optional.py/)

An Implementation of the Optional Object for Python

## Why

There is a difference between `None` as Empty and `None` as the result for an Error.  A common bad practice is to
return `None` to indicate the absence of something. Doing this introduces ambiguity into you code.

For example:
```python
thing = stuff.getSomeThing().getAnotherThing()
```
What will happen if the result from getSomeThing returns `None`?  We will get an `AttributeError: 'NoneType' object has
no attribute 'getAnotherThing'`.

What can you do to prevent these kinds of exceptions?  You can write defensively:
```python
something = stuff.getSomeThing()
if something is not None:
    thing = something.getAnotherThing()
```
However, if we add to our chain, you can imagine how the nesting of defensive checks adds up quickly. These defensive checks obfuscate our actual business logic, decreasing readability.
Furthermore, defensive checking is an error prone process, because it is easy to forget to check a required condition.

So we present you with an **Optional** object as an alternative.

## Install

**Compatible with both python 2 and 3!**

```bash
$ pip install optional.py
```

## Usage

1. You can import it using:
    ```python
    from optional import Optional
    ```

2. You can set it to empty:

    instead of: :scream_cat:
    ```python
    return None
    ```
    you can do: :smile_cat:
    ```python
    return Optional.empty()
    ```
    or
    ```python
    return Optional.of()
    ```

3. You can set it to have content:

    instead of: :scream_cat:
    ```python
    return "thing"
    ```
    you can do: :smile_cat:
    ```python
    return Optional.of("thing")
    ```


4. You can check if its present:

    instead of: :scream_cat:
    ```python
    if thing is not None:
    ```
    you can do: :smile_cat:
    ```python
    thing = some_func_returning_an_optional()
    if thing.is_present():
    ```

5. You can check if its empty:

    instead of: :scream_cat:
    ```python
    if thing is None:
    ```
    you can do: :smile_cat:
    ```python
    thing = some_func_returning_an_optional()
    if thing.is_empty():
    ```


6. You can get the value:

    instead of: :scream_cat:
    ```python
    print(thing)
    ```
    you can do: :smirk_cat:
    ```python
    thing = some_func_returning_an_optional()
    ...
    print(thing.get())
    ```
    **but this is not the recommended way to use this library.**

7. You **can't** get the value if its empty:

    instead of: :crying_cat_face:
    ```python
    if thing is None:
        print(None) # very odd
    ```
    you can do: :smirk_cat:
    ```python
    thing = some_func_returning_an_optional()
    if thing.is_empty():
        print(thing.get()) # **will raise an exception**
    ```
    **but this will raise an exception!**

8. **Best Usage:** You can chain on presence:

    instead of: :scream_cat:
    ```python
    if thing is not None:
        print(thing)
    ```
    you can do: :heart_eyes_cat:
    ```python
    thing = some_func_returning_an_optional()
    thing.if_present(lambda thing: print(thing))
    ```


9. **Best Usage:** You can chain on non presence:

    instead of: :scream_cat:
    ```python
    if thing is not None:
        print(thing)
    else:
        print("PANTS!")
    ```
    you can do: :heart_eyes_cat:
    ```python
    thing = some_func_returning_an_optional()
    thing.if_present(lambda thing: print(thing)).or_else(lambda _: print("PANTS!"))
    ```
    Note that the lambdas here can be swapped out for actual function names.

10. **Best Usage:** You can map a function: :heart_eyes_cat:

    ```python
    def mapping_func(thing):
        return thing + "PANTS"

    thing_to_map = Optional.of("thing")
    mapped_thing = thing_to_map.map(mapping_func) # returns Optional.of("thingPANTS")
    ```
    **Note** that if the mapping function returns `None` then the map call will return `Optional.empty()`. Also
    if you call `map` on an empty optional it will return `Optional.empty()`.

11. **Best Usage:** You can flat map a function which **already returns an Optional**: :heart_eyes_cat:
    ```python
    def flat_mapping_func(thing):
        return Optional.of(thing + "PANTS")

    thing_to_map = Optional.of("thing")
    mapped_thing = thing_to_map.map(mapping_func) # returns Optional.of("thingPANTS")
    ```
    **Note** that this does not return an Optional of an Optional.  __Use this for mapping functions which return optionals.__
    If the mapping function you use with this does not return an Optional, calling `flat_map` will raise a
    `FlatMapFunctionDoesNotReturnOptionalException`.

12. You can compare two optionals: :smile_cat:
    ```python
    Optional.empty() == Optional.empty() # True
    Optional.of("thing") == Optional.of("thing") # True
    Optional.of("thing") == Optional.empty() # False
    Optional.of("thing") == Optional.of("PANTS") # False
    ```


## Tests

There is complete test coverage and they pass in both python 2 and 3.

#### Running Unit Tests

You can run the tests using:
```bash
$ python2 -B -m unittest discover
```

or

```bash
$ python3 -B -m unittest discover
```

#### Test Coverage
if you install the requirements in `test_requirements.txt` using:
```bash
$ pip install -r test_requirements.txt
```
You can check the code coverage using:
```bash
$ coverage run -m --branch unittest discover
$ coverage report
```

