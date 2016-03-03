2.5. Flask-SQLAlchemy
=====================

[Flask-SQLAlchemy](http://flask-sqlalchemy.pocoo.org/2.1/)
will wrap SQLAlchemy for us so it's easier to work with it within a Flask app.
As always, install it (it will consequently install SQLAlchemy as well)
and refresh `requirements.txt`:

  ```bash
  (venv) $ pip install flask-sqlalchemy
  ...
  (venv) $ pip freeze > requirements.txt
  ```

Initialize and connect to app
-----------------------------

To have Flask-SQLAlchemy running, you need to provide a URI for you DB,
and create an instance of SQLAlchemy (from `flask.ext`):

  ```python
  from flask.ext.sqlalchemy import SQLAlchemy

  basedir = os.path.abspath(os.path.dirname(__file__))

  app = Flask(__name__)
  app.config['SQLALCHEMY_DATABASE_URI'] = \
      'sqlite:///' + os.path.join(basedir, 'data.sqlite')
  app.config['SQLALCHEMY_COMMIT_ON_TEARDOWN'] = True

  db = SQLAlchemy(app)
  ```

This is of course the simple way of doing things: create the app,
give the necessary configuration values, and instantiate SQLAlchemy.
The `SQLALCHEMY_COMMIT_ON_TEARDOWN` values makes automatic commits
at the end of each request. For apps created through an application factory,
things are just slightly more complicated.

This automatically handles all connection settings and, more importantly,
creates the necessary declarative class, `db.Model`, from which
all your table-classes must inherit!

Model definitions
-----------------

Create your models, then, as `db.Model` subclasses,
and columns as `db.Column` subclasses (the former's attributes).
For example:

  ```python
  class User(db.Model):
      __tablename__ = 'users'

      id = db.Column(db.Integer, primary_key=True)
      name = db.Column(db.String(64), unique=True)

      def __repr__(self):
          return '<User {0}>'.format(self.name)
  ```

A primary key is required.
`__tablename__` sets the name for the SQL table,
and `__repr__` is useful for debugging.

One-to-one or one-to-many relationships are set as foreign keys.
A backwards link can be added on the other side of the relationship:

  ```python
  # on the "many" side
  role_id = db.Column(db.Integer, db.ForeignKey('roles.id'))

  # on the "one" side
  users = db.relationship('User', backref='role')
  ```

Many-to-many relationships require an extra table, of course,
defined as a `db.Table` (not as a class themselves), and with
`db.Column`s for its foreign keys. Again, reversed references
can be created from the other side of the relationship.
If, for example, each user could have several roles:

  ```python
  user_role = db.Table('user_role',
      db.Column('user_id', db.Integer, db.ForeignKey('user.id')),
      db.Column('role_id', db.Integer, db.ForeignKey('role.id'))
  )

  class User(db.Model):
      # ...
      roles = db.relationship('Role', secondary=user_role, backref='users')
  ```
Database operations
-------------------

All DB operations are handled through the SQLAlchemy object:

* `db.create_all()` for the creation of the db tables themselves
  (when Python classes are already defined).
  In case you need to delete your database, use `db.drop_all()`
  (though we'll handle changes in structure with migrations).
* To add files, create the Python objects first, and then `db.session.add(x)`.
  This one is also used to modify existing rows.
* For deleting rows: `db.session.delete(x)`.
* After making these changes, always `db.session.commit()`.
  This isn't necessary if `SQLALCHEMY_COMMIT_ON_TEARDOWN` is set to true.

Every model class has a `query` object associated as well, which you can use:

* On brute force: `User.query.all()`.
* With filters: `User.query.filter_by(role='Guest').all()`.
* Select just one item: `User.query.filter_by(role='Guest').first()`.

Use within functions and views
------------------------------

There isn't much to add here. In a view function,
you'll probably check form data against the DB,
and/or add or update some of that data:

  ```python
  @app.route('/', methods=['GET', 'POST'])
  def index():
      form = NameForm()
      if form.validate_on_submit():
          user = User.query.filter_by(name=form.name.data).first()
          if user is None:
              user = User(name=form.name.data)
              db.session.add(user)
          session['name'] = form.name.data
          return redirect(url_for('index'))
      return render_template('index.html', form=form)
  ```

We don't include `db.session.commit()` here supposing it's automated in config.
Don't confuse `db.session`, the session with the DB, with
`session`, the Flask variable for encrypted cookies.

Forms are easily shown in templates just by calling its fields:

  ```html
  <form method="POST">
    {{ form.name.label }}: {{ form.name() }}
    {{ form.submit() }}
  </form>
  ```

DB through the shell
--------------------

Sometimes it's useful to access the DB not through the app itself,
but within a shell session (for creation/deletion, or debugging).
In order to have all the necessary variables automatically imported,
you can modify the `shell` command itself thanks to Flask-Script:

  ```python
  def make_shell_context():
      return dict(app=app, db=db, User=User)
  manager.add_command("shell", Shell(make_context=make_shell_context))
  ```

Repository
----------

For a deeper look at Flask-SQLAlchemy, check the
[source code](https://github.com/mitsuhiko/flask-sqlalchemy)
at GitHub.
