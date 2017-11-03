Converting Strings to Bytes
===========================

Most common programming languages have some way
to convert a Unicode string to bytes.
To do that, one must specify the encoding;
in the case of the IPDB Protocol, the encoding must be
`UTF-8 <https://en.wikipedia.org/wiki/UTF-8>`_.


**Example Python 3 Code**

If ``example_string`` is a Python 3 
`str object <https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str>`_
(an immutable sequence of Unicode code points),
then it can be converted to a Python 3 
`bytes object <https://docs.python.org/3/library/stdtypes.html#bytes-objects>`_
using:

.. code:: python

   example_bytes = example_str.encode()

The Python 3
`str.encode() method <https://docs.python.org/3/library/stdtypes.html#str.encode>`_
assumes a UTF-8 encoding by default.
