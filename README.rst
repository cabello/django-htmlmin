++++++++++++++
django-htmlmin
++++++++++++++

html minify for django

Why minify HTML code?
=====================

One of important points on client side optimization is minify HTML, with minified HTML code, you reduce the size of data transferred from your server to your client, and your pages load faster.

Installing
==========

For install django-htmlmin, run on terminal: ::

    $ [sudo] pip install django-htmlmin

Using the midleware
===================

All you need to do is add ``htmlmin.middleware.HtmlMinifyMiddleware`` to your ``MIDDLEWARE_CLASSES``: ::

    MIDDLEWARE_CLASSES = (
        # other middleware classes
        'htmlmin.middleware.HtmlMinifyMiddleware',
    )

Excluding some URLs
-------------------

If you don't want to minify all views in your app and it's under a ``/my_app`` URL, you can tell the middleware to not minify the response of your views by adding a ``EXCLUDE_FROM_MINIFYING`` setting on your settings.py: ::

    EXCLUDE_FROM_MINIFYING = ('^my_app/', '^admin/')

As you can see, you use a regex pattern for URL exclusion. If you want to exclude all URLs of your app, except a specific view, you can use the decorator ``minified_response`` (check the next section above).

Using the decorator
===================

django-htmlmin also provides a decorator, that you can use only on views you want to minify the response: ::

    from htmlmin.decorators import minified_response

    @minified_response
    def home(request):
        return render_to_response('home.html')

Using the html_minify function
==============================

If you are not working with Django, you can invoke the ``html_minify`` function manually: ::

    from htmlmin.minify import html_minify
    html = '<html>    <body>Hello world</body>    </html>'
    minified_html = html_minify(html)

Here is an example of `Flask <http://flask.pocoo.org>`_ view: ::

    from flask import Flask
    from htmlmin.minify import html_minify

    app = Flask(__name__)

    @app.route('/')
    def home():
        rendered_html = render_template('home.html')
        return html_minify(rendered_html)

Keeping comments
----------------

By default, ``html_minify`` function removes all comments. If you want to keep them, you can pass ``False`` as value to ``ignore_comments`` parameter on that function: ::

    from htmlmin.minify import html_minify
    html = '<html>    <body>Hello world<!-- comment to keep --></body>    </html>'
    minified_html = html_minify(html, ignore_comments=False)


Using command line tool
=======================

If you are not even using Python, you can use the ``pyminify`` command line tool to minify HTML files: ::

    $ pyminify index.html > index_minified.html

You can also keep comments, if you want: ::

    $ pyminify --keep-comments index.html > index_minified_with_comments.html

development
===========

* Source hosted at `GitHub <http://github.com/cobrateam/django-htmlmin>`_
* Report issues on `GitHub Issues <http://github.com/cobrateam/django-htmlmin/issues>`_

Pull requests are very welcomed! Make sure your patches are well tested.

running the tests
-----------------

if you are using a virtualenv, all you need is:

::

    $ make test

community
=========

irc channel
-----------

#cobrateam channel on irc.freenode.net
