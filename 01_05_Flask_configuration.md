1.5. Flask configuration
========================

This is mainly a comment on
[this page](http://flask.pocoo.org/docs/0.10/config/)
from the official documentation.
So, for more info, check that one instead.

Basics
------

Every application needs a set of configuration values
separated from the code itself. Why separated?

* Pure modularization: one place for the code, another for the config.
* You probably need more than one configuration, generally three:
  one for development, one for testing and one for production.
  This way it's easier to change between them as needed.
* You may use version control, but you don't want certain information
  (like secret keys) to be publicly displayed alongside your code!

How does Flask handle this? Quite simply,
the `Flask` object has an attribute `config` (a dictionary)
that stores all configuration variables. All variables must be uppercase.
Some common variables are set by Flask itself, with default values.
Some basic examples:

* **DEBUG**: enables debugging, used by every single sensible human
  in development (but not in production).
* **TESTING**: similarly, enables testing mode.
* **SECRET_KEY**: needed for many things related to encryption (like cookies).

Patterns for loading configuration
----------------------------------

You can load configuration sets from:

* An environment variable pointing to a file:
  `app.config.from_envvar('SETTINGS')`.
* An object, generally a class holding the variables:
  `app.config.from_object('Config')`.

This leads to the two common patterns for handling configuration:

* Have some default settings, and then load config from an environment variable
  (thus properly having environment-dependent values).
* Use class inheritance to manage different config settings,
  and import from one or another in each case.
