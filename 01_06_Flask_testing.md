1.6. Flask testing
==================

Suggested unit testing framework in the Flask documentation is
Python's `unittest` [module](https://docs.python.org/2/library/unittest.html).
You can also check Flask's [docs](http://flask.pocoo.org/docs/0.10/testing/)
on the subject. Here follows a brief description.

unittest
--------

The module is built on four main concepts:

* **Test fixture**: the preparation necessary before running the tests,
  and the cleanup needed after them. They will be called for each...
* **Test case**: the smallest unit of testing.
* **Test suite**: a collection of test cases.
* **Test runner**: this speaks for itself.

`unittest` provides a base `TestCase` class, which you should subclass.
Inside it, the `setUp` and `tearDown` functions will be executed as fixture.
Any method starting with `test_` will be interpreted as a test to do.

Within these methods, use any of the assert methods provided:
`assertEqual`, `assertNotEqual`, `assertTrue`, `assertFalse`.
`assertRaises` is usually used as a with statement:

  ```python
  with self.assertRaises(exception):
      callable()
  ```

Testing in Flask
----------------

Within Flask, the use of this module is no different:

* Create a different module or package for testing, and subclass `TestCase`.
* Define your fixture, which will include app (with testing config)
  and temporary database creation.
* Define your tests for any single feature you implement.

Remember that unit testing is better handled when done parallel to development!
