2.1. Flask-Script
=================

[Flask-Script](http://flask-script.readthedocs.org/en/latest/)
is an extension that adds a command-line parser to your app.
This can be used to change configuration options, set up your database,
actually running the development server, etc.
These uses will show themselves when the time comes.
Meanwhile, let's install it.

Extensions installation and connection
--------------------------------------

Install Flask-Script just as any other library or Flask extension:

  ```bash
  (venv) $ pip install flask-script
  ```

Remember that your virtual environment must be activated so that
the extension gets installed on the proper Python interpreter.
Every time you install a new library, remember to refresh the requirements:

  ```bash
  (venv) $ pip freeze > requirements.txt
  ```

Now that it's installed, you have to connect it to your app.
This strategy (within `manage.py`) is always used for Flask extensions:

  ```python
  from app import create_app
  from flask.ext.script import Manager #1

  app = create_app('default')
  manager = Manager(app) #2

  if __name__ = '__main__':
      manager.run()
  ```

Flask extensions are exposed through the `flask.ext` namespace.
You will always import the main class from the extension (#1), and then
instantiate it, passing the application as its argument (#2).

Flask-Script usage
------------------

Now that we have Flask-Script connected to our app, running the server
will be handled through this extension. If you run this in the shell:

  ```bash
  (venv) $ python manage.py
  ```

you'll see some help, which tells you that you can use two default commands:
`shell`, which starts a Python shell session in the context of the app, and
`runserver`, which... starts the server!

The first advantage is that `runserver` has many options on its own
(like host and port selection). Just type `python manage.py runserver --help`.
The second and crucial advantage is that you can add your own custom commands:

  ```python
  @manager.command
  def hello():
      print "Hello World!"
  ```

This simple decorator and function add a `hello` command that prints a message.
More complex commands can be added, and there's where Flask-Script
fully shows its usefulness. Later we will take advantage if this.

Repository
----------

For a deeper look at Flask-Script, check the
[source code](https://github.com/smurfix/flask-script)
at GitHub.
