2.4. SQLAlchemy
===============

[SQLAlchemy](http://www.sqlalchemy.org/)
([docs here](http://www.sqlalchemy.org/docs/))
is the most versatile SQL toolkit for Python.
Every conceivable app will have some sort of database.
And the standard language for the classic
[relational model](https://en.wikipedia.org/wiki/Relational_model)
is, of course, [SQL](https://en.wikipedia.org/wiki/SQL).
Some apps may need [NoSQL](https://en.wikipedia.org/wiki/NoSQL) models,
but for our app we will use a relational database.

SQLAlchemy will create an abstraction layer, called
[ORM](https://en.wikipedia.org/wiki/Object-relational_mapping)
or object-relational mapping, so that you can work directly
on high-level Python classes and objects. As a consequence,
it will handle the differences between MySQL, Postgres and SQLite.

Expression language and ORM
---------------------------

As a complex and flexible tool, SQLAlchemy offers two interfaces to DBs.
The low-level approach or
[Core](http://docs.sqlalchemy.org/en/rel_1_0/core/tutorial.html)
is called _SQL Expression Language_,
and is useful whenever you need precise control over DB handling.
The high-level, on which we'll concentrate here, is the
[ORM](http://docs.sqlalchemy.org/en/rel_1_0/orm/tutorial.html)
itself, enough for most basic purposes.

The docs describe them as working from the perspective
of the DB schema itself, or of the domain model, respectively.
Of course, both layers can be freely mixed if necessary.

ORM basics
----------

The basic steps you need to take are as follows:

* Create an `Engine` instance and connect it to your database.
* Define a **declarative base class**, which will maintain a catalog
  of all your classes and tables.
* Define your DB tables, on one hand, and your Python classes, on the other,
  and map the latter to the former as appropriate.
  All these classes must inherit from your declarative class.
  This whole point is in fact a one step process, called a declarative system.
* All classes expect a `__tablename__` attribute,
  which will be set as the corresponding table's name.
  They also require at least one column attribute.
* Everything, of course, inherits from SQLAlchemy's base classes,
  `Table` and `Column`.
* When your tables, classes and mapping are defined, you have to
  `Base.metadata.create_all(engine)`.

For handling operations with the database rows,
you wil need a `Session` instance bound to your engine.
Then:

* Add an entry by calling the class: `u1 = User(name="John")`.
* Or query with the session's `query` method
  (each query can then be filtered, ordered or grouped).
* Updating can bee done just by assigning the object's variables.
* You must then `session.add(u1)` or `session.delete(u1)`.
* For changes to be actually made on the database, `session.commit()`;
  or `session.rollback()` if necessary.

One-to-may or one-to-one relationships are easily handled:
just add a `ForeignKey` column on the 'many' side,
and a `relationship` column on the 'one' side.
Many-to-many relationships are handled with a relational `Table`
that stores foreign keys to both 'many' tables.
Examples can be found on the next section on Flask-SQLAlchemy, or on the
[docs](http://docs.sqlalchemy.org/en/rel_1_0/orm/basic_relationships.html).

Repository
----------

For a deeper look at SQLAlchemy, check the
[source code](https://github.com/zzzeek/sqlalchemy)
at GitHub.
