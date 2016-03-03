1.8. Structuring Flask apps
===========================

The one-file model used as a starting point becomes too limited
once your project starts to grow beyond the _Hello World!_ realm.
To avoid future problems, it's always a good idea to have
a sensible structure from the very beginning of any project.

Base structure
--------------

Check [the docs](http://flask.pocoo.org/docs/0.10/patterns/packages/)
for the source of this advice on structuring.
Start with a folder tree such as this one:

  ```
  /yourapplication
    /app
      __init__.py
      views.py
      errors.py
      /static
      /templates
    /tests
      __init__.py
    /venv
      ...
    manage.py
    config.py
    README.md
    requirements.txt
    .gitignore
  ```

The package strategy will make modularization much easier.
So, in the root folder you have:

* `manage.py`, for running the server (and some other future stuff).
  This is necessary for you can't run it from the `__init__.py` file
  (Python won't let you use a module inside a package as a startup).
* `config.py`, for managing your different configuration settings.
* `README.md`, the standard Python "start here" doc file.
* `requirements.txt`, a file that lists all dependencies for your project.
  This is usually created with `pip freeze > requirements.txt`.
  It's required by many servers for them to know what packages to install.
* `.gitignore`, if you're using [Git](http://git-scm.com/) (which you should).
  Here you will list everything you don't want Git to consider
  (such as your `/venv` folder).

Then you'll have different packages for your app and for testing.
Your `/app` folder will store static and templates files,
both in their Flask default folders.

We also added separate Python modules for common and error views.
You may think that these could be written directly within `app/__init__.py`.
But it's more that possible that things get complicated,
so it might be better to modularize just from the beginning.

Blueprints
----------

Flask provides a tool called
[blueprints](http://flask.pocoo.org/docs/0.10/blueprints/)
to better handle large apps and modularization.
A blueprint is mostly like an application itself, just that it isn't.
It's an object used to register some functionality, views, resources, etc.

The common use cases include:

* Factor an application into a set of blueprints.
* Register a blueprint on a path prefix or subdomain.
* Register the same blueprint several times
  (under different prefixes/subdomains).
* Collect a set of templates, static files and other resources
  under a blueprint (may be combined with the former uses).

Using blueprints implies several steps:

* Create the blueprint, usually from an `__init__.py` file
  within a package used for that blueprint.
  You create it as an instance of the Blueprint class:
  `admin = Blueprint('admin', __name__)`.
* If you need to, expose static and/or template folders
  through the blueprint, at creation time
  (they will be added to the regular app folders, with lower priority):
  `admin = Blueprint('admin', __name__, template_folder='template')`.
* Add URL rules (and error handlers) to the blueprint,
  using the same syntax you use with regular app routing:
  `@admin.route('/moderate')`.
* Register the blueprint on the application.
  If you want to add an URL prefix, do it now:
  `app.register_blueprint(admin, url_prefix='/admin')`.

Keep in mind that endpoints change to include the blueprint's.
In the routing example above, the endpoint would change
to something like `admin.moderate`.

Application factory
-------------------

The concept of
[Application Factory](http://flask.pocoo.org/docs/0.10/patterns/appfactories/)
implies no more that moving the creation of Flask object into a function.
Why? Specially for testing, so that you can create different instances
of the app with different configuration settings.

The function looks something like this:

  ```python
  def create_app(config):
      app = Flask(__name__)
      app.config.from_object(config)

      from yourapplication.models import db
      db.init_app(app) #1

      from yourapplication.admin import admin
      app.register_blueprint(admin, url_prefix='/admin')

      return app
  ```

Databases (and extensions in general) will be covered later,
but the formula at #1 is a necessary workaround:
`db` will have been created outside the function,
and so unbound to the app; then you have to register it.

Once this function is defined, you can create and run the app as expected:

  ```python
  app = create_app(config)
  app.run()
  ```

Be aware that, since we moved the app creation forward,
you can't access the Flask object itself anywhere.
That affects routing, for example (`@app.route('/')`),
and so the blueprint solution comes in handy, because it lets you define
the routes and only later register them on the app when it's created.
It also affects configuration variable access, for example.
That's where `current_app` shows its usefulness!

Structure revisited
-------------------

The addition of blueprints and app factories
imply some small changes to our project structure and code.
The ones on the code have been pointed out.
On the structure, just some small changes have to be made
in order to encapsulate each blueprint in its own package.

We sketch here a possible structure with two blueprints,
`main` and `user` (only the `/app` folder is shown).
Each blueprint might have their own views, static and template folders, etc.

  ```
  /yourapplication
    /app
      __init__.py
      /static
      /templates
      /main
        __init__.py
        views.py
        errors.py
        /templates
      /user
        __init__.py
        views.py
        /templates
    ...
  ```

With this base to work from, it should be easier
to scale properly to more complex projects.
