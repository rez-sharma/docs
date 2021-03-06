.. _sample-data-block:

Sample data block
=================

This is a complete data block example.

The full layout on disk would be:

::

    data-block/
        tiles/      # Data files that the block serves are here
        Dockerfile
        UP42Manifest.json
        run.py

.. _sample-data-block-download:

:download:`Download the complete example <data-block.zip>`

Building the block
------------------

.. code-block:: bash

    $ wget http://docs.up42.com/_downloads/d51c1a173014e2a1a976ab6697d54bf9/data-block.zip
    $ unzip data-block.zip
    $ cd data-block
    $ docker build . -t data-block --build-arg manifest="$(cat InterstellarManifest.json)"

For more information, see the :ref:`guide on building and pushing blocks <build-and-push-first-block>`.

Source code
-----------

Dockerfile
++++++++++

.. literalinclude:: sample/Dockerfile
   :language: docker

Manifest
++++++++

.. literalinclude:: sample/UP42Manifest.json
   :language: javascript

Python source code
++++++++++++++++++

.. literalinclude:: sample/run.py
   :language: python
