2.3. Flask-WTF
==============

[Flask-WTF](https://flask-wtf.readthedocs.org/en/latest/)
wraps WTForms to easier use within Flask.
As already shown with the case of Flask-Script, you should first install it:

  ```bash
  (venv) $ pip install flask-wtf
  ```

This will install WTForms as well, as it's a dependency.
Both libraries must be added again to `requirements.txt`.

Basic usage
-----------

The two key steps you always need to take are:

* To do the specific imports: the `Form` class, _from the flask extension_,
  and all the fields, validators, etc. _from WTForms itself_.
* To use
  [CSRF Protection](https://en.wikipedia.org/wiki/Cross-site_request_forgery),
  for which you have to give a `SECRET KEY` on your configuration
  (you'll need that key anyway for some other matters).
  Then add the necessary token _on every form_ with `{{ form.hidden_tag() }}`.

And then, a three step process. First, create the form classes
(usually within an specific `forms.py` module). A simple example:

  ```python
  from flask.ext.wtf import Form
  from wtforms import StringField, SubmitField

  class NameForm(Form):
      name = StringField('Name')
      submit = SubmitField('Submit')
  ```

Second, you instantiate the form in the proper view:

  ```python
  from forms import NameForm

  @main.rounte('/', methods=['GET', 'POST'])
  def index():
      form = NameForm()
      if form.validate_on_submit():
          # ...
      return render_template(index.html, form=form)
  ```

The `form.validate_on_submit()` is a practical method for checking both
that the method is POST and that the form validates.
Check the section on WTForms and this library's documentation
to see how to implement validators for your forms.

Third, you call the fields from your template:

  ```html
  <form method="POST">
    {{ form.hidden_tag() }}
    {{form.name.label }}: {{ form.name() }}
    {{ form.submit() }}
  </form>
  ```  

Repository
----------

For a deeper look at Flask-WTF, check the
[source code](https://github.com/lepture/flask-wtf)
at GitHub.
