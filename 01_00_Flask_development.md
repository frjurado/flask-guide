1. Flask Development
====================

Here I pretend to document the creation of a blog using the
[Flask](http://flask.pocoo.org/) microframework.
Flask is a Python framework for developing web apps.
It's designed as a very basic core which can then be extended.

We will cover Flask essentials in this chapter.
As it is based on two other libraries, Werkzeug and Jinja2,
we will briefly talk about those too.

The second chapter will cover the basic functionalities
every reasonable app should implement (such as DB, forms or email).
As already hinted, we'll have to use extensions for these.

Subsequent chapters will deal with the specifics of a blog application:
users, roles, posts, tags and comments.

Repository
----------

We will travel then from the most general to the most specific.
We do it that way so the documentation can be forked easily
to be applied to other kinds of apps.

Moreover, a repository will be referenced with versions corresponding to
each of these steps, so that you can fork it too for your own purposes.

So, let's dive in right away.
