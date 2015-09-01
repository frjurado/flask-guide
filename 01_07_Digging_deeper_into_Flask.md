1.7. Digging deeper into Flask
==============================

But before we go on to a better and more complex structure
for our Flask project, there are some design subtleties to cover.
So let's take a small detour.
(For more information, follow these links to the docs on the
[Application](http://flask.pocoo.org/docs/0.10/appcontext/) and
[Request](http://flask.pocoo.org/docs/0.10/reqcontext/) Contexts.)

Application Context
-------------------

Flask is designed to work in three different states:

* The **setup state**,
  from Flask object initialization to the first request.
  Here you can still modify the object.
* The **request state**,
  during which some variables (like `request`) become globally available.
* A **pseudo-request state**,
  used sometimes during shell sessions or testing.
  Here you might want to access some app data,
  though there is no actual request active.

Flask always had a Request Context (see below).
To handle these complex cases, though, they created the App Context.
It is created either implicitly (whenever a Request Context is created)
or explicitly:

  ```python
  from flask import Flask
  app = Flask(__name__)

  # Option 1 (more useful with fized code)
  with app.app_context():
      ### do stuff

  # Option 2 (equivalent, more useful in the shell)
  app_ctx = app.app_context()
  app_ctx.push()
  ### do stuff
  app_ctx.pop()
  ```
During the lifespan of an App Context, the variable `current_app`
points to... just that, the current active app object.
`g` is the other variable created by the context,
which is used for storing any user data.

Request Context
---------------

This context is again implicitly created whenever a request comes in,
but you can also create it explicitly if needed
(`app.test_request_context(url)` is the equivalent method).

During the Request Context lifespan,
the variables `request` and `session` are made globally available.

The request cycle is fully explained with these extra functions:

* `before_request()` functions are called first.
  They can return a Response object, in which case
  the next step is eluded. If they don't:
* The regular view function is called, which returns a Response.
* Either response (from `before_request()` or regular view function)
  is sent to `after_request()` functions, which can further process it.
* In any case, even if an error occurs, after the Request Context is popped
  `teardown_request()` functions are called.

Keep in mind that, if you're working with the shell and create
the Request Context explicitly, you still have to manually call
before and after request (but not the teardown) functions.

  ```python
  >>> req_ctx = app.test_request_context('/')
  >>> req_ctx.push()
  >>> app.preprocess_request() # before_request functions
  >>> response = index()
  >>> app.process_request(response) # after_request functions
  >>> req_ctx.pop() # teardown_request() is automatically called
  ```

Flask contexts and related variables are somewhat confusing,
but their common uses (think of the `request` object) are simple enough.
Some complex cases, though, make this theoretical detour necessary,
as blueprints and app factories might show.
