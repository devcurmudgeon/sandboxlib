The 'sandboxlib' library uses the PEP-8 coding style, as a guide.

Release process
---------------

Basically, tag the commit you're going to release, build and upload a source
distribution tarball to PyPI_, and then increment the version number.

You need
- an account on PyPI with access to the 'sandboxlib' project
- push access to https://github.com/codethinklabs/sandboxlib

Process:

1. Run tests: ``sudo tox``
2. Create source distribution tarball: ``python ./setup.py sdist``
3. Upload to PyPI: ``twine upload -u $PYPI_USERNAME -p $PYPI_PASSWORD dist/sandboxlib-0.0.0.tar.gz``
4. Create and push tag: ``git tag --annotate -m "sandboxlib version 0.0.0" 0.0.0 && git push --tags``

I intend to follow `semantic versioning`_ and `PEP 440`_ when numbering
versions of this library.

For background on realising to the Python Package Index (PyPI), see:
https://packaging.python.org/en/latest/distributing.html.

.. _semantic versioning: <http://www.semver.org/>
.. _PEP 440: https://www.python.org/dev/peps/pep-0440/
.. _PyPI: http://pypi.python.org/

Running the automated test suite
--------------------------------

Use ``tox``. You'll need 'py.test', 'tox' and their dependencies available.

Note that a lot of the tests will be skipped or fail if you don't run as
'root', because some of the sandboxing backends only work when you are the
'root' user. The test suite could handle this better than it does.

You can also run ``PYTHONPATH=. py.test``, which is quicker but only tests with
a single version of Python, and runs in your host environment rather than a
clean one managed by 'virtualenv'.

Testing that a sandbox conforms to the App Container spec
---------------------------------------------------------

The `App Container project`_ provides an 'ace' package containing a
`validator application for App Container Executors (ACEs)`_.

If you want to test whether a particular sandbox 'exec' module conforms to the
App Container spec, try this. You'll need a golang_ compiler available, and you
might need to set the ``GOPATH`` environment variable to point to a path where
you're happy for Go to install dependencies.

First build the ``ace-validator-main.aci`` and ``ace-validator-sidekick.aci`` App
Container images::

    git clone git://github.com/appc/spec appc-spec
    cd appc-spec
    ace/build
    cd -

Then, use the ``run-sandbox`` program from this repository to run the image::

    run-sandbox appc-spec/ace/build/ace-validator-main.aci -e linux-user-chroot


.. _App Container project: https://github.com/appc/spec
.. _validator application for App Container Executors (ACEs)`: https://github.com/appc/spec#validating-app-container-executors-aces
.. _golang: https://golang.org/doc/install