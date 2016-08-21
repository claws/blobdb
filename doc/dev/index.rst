Developers Guide
================

The project is hosted on `GitHub <https://github.com/claws/digestdb>`_.
and uses `Travis <https://travis-ci.org/claws/digestdb>`_ for
Continuous Integration.

If you have found a bug or have an idea for an enhancement that would
improve the library, use the
`bug tracker <https://github.com/claws/digestdb/issues>`_.


Setup
-----

The best way to work on digestdb is to create a virtual env. This
isolates your work from other project's dependencies and ensures that any
commands are pointing at the correct tools.

.. note::

    In the following example ``python`` is assumed to be the Python 3.5
    executable. You may need to explicitly specify this (e.g. use ``python3``)
    if you have multiple Python's available on your system.

.. code-block:: console

    $ python -m venv myvenv
    $ cd myvenv
    $ source bin/activate
    $ cd ..

To exit the virtual environment simply type ``deactivate``.

.. note::

    The following steps assume you are operating in a virtual environment.


Get the source
--------------

If you don't care about contributing back and just want the source then use:

.. code-block:: console

    $ git clone git@github.com:claws/digestdb.git

If you want to contribute changes back to the project you should first create a
fork of the digestdb repository and then clone the repo to your file-system.
Replace ``USER`` with your Github user name.

.. code-block:: console

    $ git clone git@github.com:USER/digestdb.git


Install Dependencies
--------------------

Install the developmental dependencies using ``pip``.

.. code-block:: console

    $ cd digestdb
    $ pip install -r requirements.dev.txt


Install digestdb
----------------

Use ``pip`` to perform a development install of digestdb. This installs
the package in a way that allows you to edit the code after its installed so
that any changes take effect immediately.

.. code-block:: console

    $ pip install -e .


Test
----

The easiest method to run all of the unit tests is to run the ``make test``
rule from the top level directory. This runs the standard library
``unittest`` tool which discovers all the unit tests and runs them.

.. code-block:: console

    $ make test

Or, you can call the standard library unittest module directly.

.. code-block:: console

    $ python -m unittest discover -s tests -v

Individual unit tests can be run using the standard library ``unittest``
package too.

.. code-block:: console

    $ cd digestdb/tests
    $ python -m unittest test_database_db_dir


Type Annotations
----------------

The code base has been updated with type annotations. These provide helpful
gradual typing information that can improve how easily the code is understood
and which helps with any future enhancements.

The type annotations checker ``mypy`` currently runs cleanly with no warnings.

Use the Makefile convenience rule to check no issues are reported.

.. code-block:: console

    $ make check_types


Documentation
-------------

To rebuild the project documentation, developers should run the ``make docs``
rule from the top level directory. It performs a number of steps to create
a new set of `sphinx <http://sphinx-doc.org/>`_ html content.

.. code-block:: console

    $ make docs

To quickly view the rendered docs locally as you are working you can use the
simple Python web server.

.. code-block:: console

    $ cd docs
    $ python -m http.server

Then open a browser to the `docs <http://localhost:8000/_build/html/index.html>`_
content.


.. _version-label:

Version
-------

``digestdb`` uses a three segment `CalVer <http://calver.org/>`_ versioning
scheme comprising a short year, a zero padded month and then a micro version.
The ``YY.MM`` part of the version are treated similarly to a SemVer major
version. When backwards incompatible or major functional changes occur the
``YY.MM`` will be rolled up. For all other minor changes only the micro part
will be incremented.


Release Process
---------------

The following steps are used to make a new software release:

- Update the version label in ``__init__.py``. It must comply with the
  :ref:`version-label` scheme.
- Create the distribution

  .. code-block:: console

      make dist

- Test distribution in ``dist/`` directory. This involves creating a virtual
  environment, installing the source distribution in it and running the tests.
  These steps have been captured for convenience in the ``dist/test.bash``
  helper script. The script takes the distribution archive as its only
  argument.

  .. code-block:: console

      cd dist
      ./test.bash digestdb-16.08.01.tar.gz
      cd ..

- Build the docs and check for any errors.

  .. code-block:: console

      make docs

- Upload to PyPI using

  .. code-block:: console

      make dist.upload

or manually using:

  .. code-block:: console

      python setup.py sdist upload

- Create and push a repo tag to Github.

  .. code-block:: console

      git tag YY.MM.MICRO -m "A meaningful release tag comment"
      git tag  # check release tag is in list
      git push --tags origin master

  - Github will create a release tarball at:

    ::

        https://github.com/{username}/{repo}/tarball/{tag}.tar.gz

