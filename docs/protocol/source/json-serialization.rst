JSON Serialization & Deserialization
====================================

In the IPDB Protocol, "JSON serialization" is the standard process
to convert an associative array (such as a Python dict)
to a standard Unicode JSON string. "JSON deserialization" is the reverse.
Some constraints are imposed on the JSON string:

- All keys must be strings
- The string is Unicode (not just ASCII)
- In the JSON string, all keys are sorted by key name

There are several JSON standards, notably RFC 7159 and ECMA-404.

For JSON serialization and deserialization,
the IPDB Protocol follows what RapidJSON does.
`RapidJSON <https://github.com/Tencent/rapidjson>`_
is a fast C++ JSON library.
According to `the RapidJSON documentation <http://rapidjson.org/>`_,
"RapidJSON should be in full compliance with RFC7159/ECMA-404,
with optional support of relaxed syntax."

Most common programming languages have one or more libraries/packages
which can do the same as RapidJSON.


**Using Python 3**

There's a Python 3 wrapper around RapidJSON called 
`python-rapidjson <https://github.com/python-rapidjson/python-rapidjson>`_.
In Python, associative arrays are implemented as dictionaries.
To convert a dictionary to a standard Unicode JSON string
(Python 3
`str object <https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str>`_,
standard in the sense of the IPDB Protocol):

.. code:: python

   import rapidjson

   # data is a dictionary
   json_str = rapidjson.dumps(data,
                              skipkeys=False,
                              ensure_ascii=False,
                              sort_keys=True)

- ``skipkeys=False`` ensures all keys are strings. If they're not, the serialization will fail.
- ``ensure_ascii=False`` allows non-ASCII Unicode characters to pass through into the resulting Python 3 `str object <https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str>`_.
- ``sort_keys=True`` ensures the JSON output is sorted by key.

The python-rapidjson documentation has a
`page about the dumps() function <https://python-rapidjson.readthedocs.io/en/latest/dumps.html>`_.

To deserialize a standard Unicode JSON string to a dictionary:

.. code:: python

   new_dict = rapidjson.loads(json_str)

