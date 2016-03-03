2.6. Alembic
============

[Alembic](https://alembic.readthedocs.org/en/latest/)
is a database migration framework developed by SQLAlchemy's main developer.
It works on top of that library and serves as a kind of version control
for handling changes in a database structure.

Basics of Alembic usage
-----------------------

After installing, the first thing you have to do to use Alembic is
to create a _migration environment_, that is, a directory that will store
the necessary scripts for each version change, etc.
To do so, just `alembic init <dirname>` within your app folder.

Then changing your database is actually a two-step process:

* Create a migration script, either:
  * **manually**, with `alembic revision`.
    This will generate an empty script that you have to populate
    with the necessary functions, giving instructions for table
    and column addition or deletion, etc.
    Details on how to do this can be found
    [here](https://alembic.readthedocs.org/en/latest/ops.html).
  * **automatically**, with `alembic revision --autogenerate`.
    This looks for changes on your models and guesses the necessary
    changes in the database, so writing the functions for you.
    Beware that automatic migrations are not perfect,
    so **you should always revise the scripts**.
* Apply it forward (with `alembic upgrade`)
  or backwards (with `alembic downgrade`).
  These directives have many optional parameters.
  For example, by default they go up/down one single step,
  but you can also `alembic upgrade head` (to the most recent state)
  or `alembic downgrade base` (back to nothing).
  Those are the two functions that, for each change in your database,
  you should have defined in the script.

Repository
----------

For a deeper look at Alembic, check the
[source code](https://github.com/zzzeek/alembic)
at GitHub.
