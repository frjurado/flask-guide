2.2. WTForms
============

[WTForms](http://wtforms.readthedocs.org/en/2.1/)
is a framework-agnostic Python package to handle web forms. Why do so?
So you can easily handle certain repetitive tasks related to forms.
The most obvious examples being, of course, HTML rendering
(both of forms themselves and errors) and form validation.

WTForms basics
--------------

What this utility does, in essence, is to encapsulate `Form` and `Field`
utilities within those classes, which you can then subclass for your app.
Different `Field` subclasses are already provided for different input types,
and a list of common `validators` are also given and can be used for any field.

A form is then defined with some code like this:

  ```python
  from wtforms import Form, StringField, PasswordField, validators

  class LoginForm(Form):
      username = StringField('Username', [validators.Length(min=4)])
      password = PasswordField('Password', [validators-Length(min=8)])
  ```

You can of course use subclassing yourself for better code.

To use this form, just instantiate it:

  ```python
  def login():
      form = LoginForm()
      if request.method == 'POST' and form.validate():
          # check against DB
          if user:
              redirect('index')
      return render_template('login.html', form=form)
  ```

And render it:

  ```html
  <form method="POST">
    <div>{{ form.username.label }}: {{ form.username() }}</div>
    <div>{{ form.password.label }}: {{ form.password() }}</div>
  </form>
  ```

As you see, form fields are easily rendered in HTML just by calling them.
That way you can easily add custom properties. Errors are just as easily
displayed either field-wise (`form.field.errors`) or form-wise (`form.errors`).
It's a good idea to handle error displaying in a separate macro.

Adding common validators is fairly easy,
and you can also define your custom ones.

Repository
----------

For a deeper look at WTForms, check the
[source code](https://github.com/wtforms/wtforms)
at GitHub.
