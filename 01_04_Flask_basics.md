1.4. Flask basics
=================

If you want to get a sense of how Flask works, you **should** have a look at
the [quickstart](http://flask.pocoo.org/docs/0.10/quickstart/)
and the [tutorial](http://flask.pocoo.org/docs/0.10/tutorial/).
Just a few notes.

Hello World application
-----------------------

If you have a look at the `hello.py` example, notice its three essential parts:

* The app object (instance of `Flask`) initialization:

  ```python
  from flask import Flask
  app = Flask(__name__)
  ```

* One or more view functions, decorated to form the URL map.
  Optionally, URL rules can include variables, which are passed as arguments:

  ```python
  @app.route('/')
  def index():
      return "Hello World!"

  @app.route('/user/<name>')
  def user(name):
      return "Hello {0}!".format(name)
  ```

* The server running:

  ```python
  if __name__ == '__main__':
      app.run(debug=True)
  ```

Flask as a Werkzeug wrapper
---------------------------

* Decorators add the functions to the `Map` object that handles URL routing.
  Inversely, you can construct URLs with the `url_for` function.
* Though the example doesn't show it, a `request` object
  wraps the corresponding Werkzeug object, through which
  you can access query parameters, form values, etc., just as in Werkzeug.
* The functions use the return values to construct `Response` objects.
  They are **200 OK** by default, though you can change that behavior.
  Special functions are given to construct common
  **302 Found** (`redirect`) and
  **404 Not Found** (`abort`) responses.
* The `run` method wraps Werkzeug's `run_simple`.

Flask as a Jinja2 wrapper
-------------------------

* Flask loads templates from a `/templates` folder by default.
* To render a template, just call the `render_template` function,
  with its name as a first argument, and any variables needed afterwards.
* It sets autoescaping to True for common extensions (`.html` and the like).
* It makes some extra variables globally accessible for templates:
  `config`, `request`, `session`, `g`, `url_for()` and `get_flashed_messages()`.

Some more basic information
---------------------------

* Flask expects static files to be in a `/static` folder.
  You can refer to them using the special `static` endpoint:

  ```python
  url_for('static', filename='style.css')
  ```

* Cookies can be accessed and created just as in Werkzeug, just don't.
  Flask provides a `session` object, implemented on top of cookies
  but with cryptographic protection added on top.
  In order to use it you need a `SECRET_KEY` value configured.
* Flask includes a `flash()` function that makes any message available
  on the next (and only the next) request. To retrieve it,
  user the `get_flashed_messages()` function.

Repository
----------

Check the Flask code [here](https://github.com/mitsuhiko/flask).
