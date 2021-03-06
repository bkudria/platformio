.. |PIOAPICR| replace:: *PlatformIO Library Registry Crawler*
.. _library_config:

library.json
============

``library.json`` is a manifest file of development library.

Initially it was
developed for :ref:`librarymanager`, but later was accepted by worldwide embedded
community like a **standard library specification**.

A data in ``library.json`` should be represented
in `JSON-style <http://en.wikipedia.org/wiki/JSON>`_ via
`associative array <http://en.wikipedia.org/wiki/Associative_array>`_
(name/value pairs). An order doesn't matter. The allowable fields
(names from pairs) are described below.

.. contents::

.. _libjson_name:

``name``
--------

**Required** | Type: ``String`` | Max. Length: 50

A name of the library.

* Must be unique.
* Should be slug style for simplicity, consistency and compatibility.
  Example: *Arduino-SPI*
* Title Case, Aa-z, can contain digits and dashes (but not start/end
  with them).
* Consecutive dashes are not allowed.


.. _libjson_description:

``description``
---------------

**Required** | Type: ``String`` | Max. Length: 255

The field helps users to identify and search for your library with a brief
description. Describe the hardware devices (sensors, boards and etc.) which
are suitable with it.


.. _libjson_keywords:

``keywords``
------------

**Required** | Type: ``String`` | Max. Length: 255

Used for search by keyword. Helps to make your library easier to discover
without people needing to know its name.

The keyword should be lowercased, can contain a-z, digits and dash (but not
start/end with them). A list from the keywords can be specified with
separator ``,``


.. _libjson_authors:

``authors``
-----------

*Required* if :ref:`libjson_repository` field is not defined | Type: ``Object``
or ``Array``

An author contact information

* ``name`` Full name (**Required**)
* ``email``
* ``url`` An author's contact page
* ``maintainer`` Specify "maintainer" status

Examples:

.. code-block:: javascript

    "authors":
    {
        "name": "John Smith",
        "email": "me@john-smith.com",
        "url": "http://www.john-smith/contact"
    }

    ...

    "authors":
    [
        {
            "name": "John Smith",
            "email": "me@john-smith.com",
            "url": "http://www.john-smith/contact"
        },
        {
            "name": "Andrew Smith",
            "email": "me@andrew-smith.com",
            "url": "http://www.andrew-smith/contact",
            "maintainer": true
        }
    ]


.. note::
    You can omit :ref:`libjson_authors` field and define
    :ref:`libjson_repository` field. Only *GitHub-based* repository is
    supported now. In this case
    |PIOAPICR| will use information from
    `GitHub API Users <https://developer.github.com/v3/users/>`_.


.. _libjson_repository:

``repository``
--------------

*Required* if :ref:`libjson_downloadurl` field is not defined | Type: ``Object``

The repository in which the source code can be found.

Example:

.. code-block:: javascript

    "repository":
    {
        "type": "git",
        "url": "https://github.com/foo/bar.git"
    }


.. _libjson_downloadurl:

``downloadUrl``
---------------

*Required* if :ref:`libjson_repository` field is not defined | Type: ``String``

It is the *HTTP URL* to the archived source code of library. It should end
with the type of archive (``.zip`` or ``.tar.gz``).


.. _libjson_version:

``version``
-----------

*Required* if :ref:`libjson_repository` field is not defined | Type: ``String``
| Max. Length: 20

A version of the current library source code.

* Can contain a-z, digits, dots or dash.
* `Semantic Versioning <http://semver.org>`_ is recommended.
* A `CVS <http://en.wikipedia.org/wiki/Concurrent_Versions_System>`_
  revision from the latest commit. Example: ``13`` (*SVN*) or first 10
  chars of *SHA* digest ``e4564b7da4`` (*Git*).

.. note::
    You can omit :ref:`libjson_version` field and define
    :ref:`libjson_repository` field. In this case
    |PIOAPICR| will use the *CVS*-revision from the latest commit.


.. _libjson_url:

``url``
-------

*Optional* | Type: ``String`` | Max. Length: 255

Home page of library (if is different from :ref:`libjson_repository` url).


.. _libjson_include:

``include``
-----------

*Optional* | Type: ``String`` or ``Array`` |
`Glob Pattern <http://en.wikipedia.org/wiki/Glob_(programming)>`_

If :ref:`libjson_include` field is a type of ``String``, then
|PIOAPICR| will recognize it like a "relative path inside
repository/archive to library source code". See example below where the only
source code from the relative directory ``LibrarySourceCodeHere`` will be
included.

.. code-block:: javascript

    "include": "some/child/dir/LibrarySourceCodeHere"

If :ref:`libjson_include` field is a type of ``Array``, then
|PIOAPICR| firstly will apply :ref:`libjson_exclude` filter and
then include only directories/files which match with :ref:`libjson_include`
patterns.

Example:

.. code-block:: javascript

    "include":
    [
        "dir/*.[ch]pp",
        "dir/examples/*",
        "*/*/*.h"
    ]

Pattern	Meaning

.. list-table::
    :header-rows:  1

    * - Pattern
      - Meaning
    * - ``*``
      - matches everything
    * - ``?``
      - matches any single character
    * - ``[seq]``
      - matches any character in seq
    * - ``[!seq]``
      - matches any character not in seq


.. _libjson_exclude:

``exclude``
-----------

*Optional* | Type: ``String`` or ``Array`` |
`Glob Pattern <http://en.wikipedia.org/wiki/Glob_(programming)>`_

Exclude the directories and files which match with :ref:`libjson_exclude`
patterns.

.. _libjson_frameworks:

``frameworks``
--------------

*Optional* | Type: ``String`` or ``Array``

A list with compatible frameworks. The available framework types are defined in
the :ref:`platforms` section.

If the library is compatible with the all frameworks, then you can use ``*``
symbol:

.. code-block:: javascript

    "frameworks": "*"

.. _libjson_platforms:

``platforms``
-------------

*Optional* | Type: ``String`` or ``Array``

A list with compatible platforms. The available platform types are
defined in :ref:`platforms` section.

If the library is compatible with the all platforms, then you can use ``*``
symbol:

.. code-block:: javascript

    "platforms": "*"


.. _libjson_dependencies:

``dependencies``
----------------

*Optional* | Type: ``Array`` or ``Object``

A list of dependent libraries. They will be installed automatically with
:ref:`cmd_lib_install` command.

Allowed requirements for dependent library:

* ``name`` | Type: ``String``
* ``authors`` | Type: ``String`` or ``Array``
* ``frameworks`` | Type: ``String`` or ``Array``
* ``platforms`` | Type: ``String`` or ``Array``

Example:

.. code-block:: javascript

    "dependencies":
    [
        {
            "name": "Library-Foo",
            "authors":
            [
                "Jhon Smith",
                "Andrew Smith"
            ]
        },
        {
            "name": "Library-Bar",
            "frameworks": "FrameworkFoo, FrameworkBar"
        }
    ]


.. _libjson_examples:

``examples``
----------------

*Optional* | Type: ``String`` or ``Array`` |
`Glob Pattern <http://en.wikipedia.org/wiki/Glob_(programming)>`_

A list of example patterns. This field is predefined with default value:

.. code-block:: javascript

    "examples": [
        "[Ee]xamples/*/*.ini",
        "[Ee]xamples/*/*.pde"
    ]
