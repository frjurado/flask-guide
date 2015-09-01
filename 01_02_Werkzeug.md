1.2. Werkzeug
=============

[Werkzeug](http://werkzeug.pocoo.org/)
is a Python WSGI library on which Flask is built.
In other words, it will be our interface with HTTP.

A word on WSGI
--------------

[WSGI](https://en.wikipedia.org/wiki/Web_Server_Gateway_Interface)
is the standard specification for interface between web servers and
Python applications. As additional resources, you can check
[the docs](http://wsgi.readthedocs.org/en/latest/),
[the specification itself](https://www.python.org/dev/peps/pep-3333/) or
[this article](http://osdcpapers.cgpublisher.com/product/pub.84/prod.21/m.1).

Tutorial
--------

There is a very basic
[tutorial](http://werkzeug.pocoo.org/docs/0.10/tutorial/)
on the Werkzeug website, which is a good starting point.
Of course, there are many things you should learn
from the whole documentation after completing the tutorial.
But we will only mention the most crucial item here.

Request and Response
--------------------

The two key classes you will deal with when working with Werkzeug are:

* **Request**: which wraps the WSGI environ, and
* **Response**: a WSGI application itself.

They can both be reached under `werkzeug.wrappers`.
Their [implementation](http://werkzeug.pocoo.org/docs/0.10/wrappers/)
can be superficially described as follows:

* **Request** is a subclass of the simpler **BaseRequest**.
  The latter implements all the basic functionality one would expect:
  `url`, `method`, `headers`, `cookies`,
  `args` (the values from the query string),
  `form` (the values from a POST form),
  `values` (values from `args` and `form` combined),
  `files` (a list of all uploaded files), etc.
* Similarly, **Response** subclasses **BaseResponse**, which accepts
  `response` (the body of the response),  `status`, `headers`
  and others as arguments.
* For adding extra functionality to these classes, Werkzeug provides several
  mixins (such as `UserAgentMixin` or `AuthenticationMixin`)
  that you can use to build your custom classes.
  All located under `werkzeug.wrappers` as well.
* **Request** and **Response** do inherit from all available mixins!

Repository
----------

For a deeper look at Werkzeug, check the
[source code](https://github.com/mitsuhiko/werkzeug)
at GitHub.
