Glossary
========

.. glossary::
   :sorted:

   associative array
      A collection of key/value (or name/value) pairs
      such that each possible key appears at most once
      in the collection.
      In JavaScript (and JSON), all objects behave as associative arrays
      with string-valued keys.
      In Python and .NET, associative arrays are called *dictionaries*.
      In Java and Go, they are called *maps*.
      In Ruby, they are called *hashes*.
      See also: Wikipedia's articles for
      `Associative array <https://en.wikipedia.org/wiki/Associative_array>`_
      and
      `Comparison of programming languages (associative array)
      <https://en.wikipedia.org/wiki/Comparison_of_programming_languages_(associative_array)>`_

   null
      ``null`` in JavaScript, Java and C#.
      ``None`` in Python.
      ``nil`` in Ruby and Go.
      If it's a value in an associative array and you convert it to a JSON string,
      it should convert to ``null`` (with no quotes around it).
