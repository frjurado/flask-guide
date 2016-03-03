2. Basic extensions
===================

Flask was designed as a micro-framework so that you could then
broaden its functionality through extensions.
This way, decisions about how to handle databases, authentication,
forms, etc., are left out for you to make freely.

Many extensions are in fact no more than nice wrappers around common libraries,
so that handling them in the context of a Flask application is smooth and easy.
That's the case with Flask-SQLAlchemy, for example, which is a
Flask wrapper for the SQLAlchemy library.

In this chapter we will install and describe the use of
some general purpose extensions, for:

* command-line control,
* form handling,
* databases and database migration,
* and email.

These might prove useful for almost any kind of app you develop.
As stated before, only then will we develop the specifics of a blog app.
