2.8. Flask-Mail
===============

For any imaginable app, you will need some sort of email support. We will use
[Flask-Mail](https://pythonhosted.org/Flask-Mail/), which wraps the standard
[smtplib](https://docs.python.org/2/library/smtplib.html) Python package
([SMTP](https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol)
stands for _Simple Mail Transfer Protocol_).

Install it with:

  ```bash
  (venv) $ pip install flask-mail
  (venv) $ pip freeze > requirements.txt
  ```

Basic usage
-----------

You need to add some configuration values to `config.py`.
Some others (sensitive data) must be given as environment variables:

  ```bash
  (venv) $ export MAIL_USERNAME=<your_username>
  (venv) $ export MAIL_PASSWORD=<your_password>
  ```

(For Windows, but NOT if you're using bash, use `set` instead.)

The two classes you need are `Mail` (initialized with `app` as argument)
and `Message`, that will construct the messages to send.
So, the necessary steps are:

* Add the required variables to `config.py`,
  or set them as environment variables.
* Instantiate your mail object (`mail=Mail(app)`).
* Create a function for sending emails, probably in a separate mail module.
  It will create the message as a `Message` object,
  and be given both plain text and html bodies.
  Then send it through `mail.send(msg)`.
* For the construction of the message bodies themselves,
  use templates (both `.txt` and `.html`).
* Call this function from you views whenever necessary.

Check the docs for more info on attachments or unit testing.

Asynchronous email
------------------

We have added a simple tool for the email sending to be handled
in a different thread, so that we avoid the latency.
It's implemented in `mail.py`, using the standard
[threading](https://docs.python.org/2/library/threading.html)
module from Python.

For a more complex solution, try for example
[Celery](http://www.celeryproject.org/).

Repository
----------

For a deeper look at Flask-Mail, check the
[source code](https://github.com/mattupstate/flask-mail)
at GitHub.
