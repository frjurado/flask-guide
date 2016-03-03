2.7. Flask-Migrate
==================

[Flask-Migrate](http://flask-migrate.readthedocs.org/en/latest/)
is the Alembic wrapper for Flask applications that we'll be using.
As always, install and refresh the requirements file:

  ```bash
  (venv) $ pip install flask-migrate
  (venv) $ pip freeze > requirements.txt
  ```

Basic usage
-----------

We will access Flask-Migrate commands through Flask-Script.
So, within `manage.py`, add:

  ```python
  from flask.ext.migrate import Migrate, MigrateCommand

  ...
  migrate = Migrate(app, db)
  manager.add_command("db", MigrateCommand)
  ```

To create the migrations directory, just:

  ```bash
  (venv) $ python manage.py db init
  ```

Then, migration scripts are created with the `revision` (manual) or
`migrate` (automatic) subcommands. Remember to check the functions generated.
This creates the migration script, but you have yet to apply the changes
with the `upgrade` subcommand. For the first migration, this is actually
the same as `db.create_all()`.

Repository
----------

For a deeper look at Flask-Migrate, check the
[source code](https://github.com/miguelgrinberg/Flask-Migrate)
at GitHub.
