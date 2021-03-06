Bambu Webhooks
==============

Create webhooks and allow users to assign URLs to them

About Bambu Webhooks
--------------------

This package allows web apps to provide third-party integration via
webhooks. You as the developer can trigger a webhook by name, and
provide an interface whereby the user can manage the URL to post the
webhook's data to.

About Bambu Tools 2.0
---------------------

This is part of a toolset called Bambu Tools. It's being moved from a
namespace of ``bambu`` to its own 'root-level' package, along with all
the other tools in the set. If you're upgrading from a version prior to
2.0, please make sure to update your code to use ``bambu_webhooks``
rather than ``bambu.webhooks``.

Installation
------------

Install the package via Pip:

::

    pip install bambu-webhooks

Add it to your ``INSTALLED_APPS`` list:

::

    INSTALLED_APPS = (
        ...
        'bambu_webhooks'
    )

Add ``bambu_webhooks.urls`` to your URLconf:

::

    urlpatterns = patterns('',
        ...
        url(r'^webhooks/', include('bambu_webhooks.urls')),
    )

Run ``manage.py syncdb`` or ``manage.py migrate`` to setup the database
tables.

Basic usage
-----------

Register a webhook within your models.py file.

::

    from hashlib import md5
    import bambu_webhooks

    bambu_webhooks.site.register('webhook_name_',
        description = 'A description of the webhook'
    )

In the ``save()`` method for your model, trigger any webhooks that have
receivers attached, thus posting the data to the user's specified URL.

::

    def save(self, *args, **kwargs):
        ...
        bambu_webhooks.send('webhook_name_', self.author,
            {
                'id': self.pk,
                'name': self.name
            },
            md5('testproject.myapp.mymodel:%d' % self.pk).hexdigest()
        )

Better with Bootstrap
---------------------

This package, among most in the Bambu toolset is designed to work with
`Bambu Bootstrap <https://github.com/iamsteadman/bambu-bootstrap>`_, a
collection of flexible templates designed for web apps based on the
Twitter Bootstrap framework. It's not a package requirement, but it'll
mean the template structure and the context variables exposed by the
view makes a little more sense.

Todo
----

-  Allow webhooks to be categorised and/or filtered
-  Prepare for internationalisation
-  Write tests

Questions or suggestions?
-------------------------

Find me on Twitter (@iamsteadman) or `visit my blog <http://steadman.io/>`_.

.. toctree::
   :maxdepth: 1
   
   cron
   menus
   models

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
