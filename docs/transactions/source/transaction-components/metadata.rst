Metadata
========

User-provided transaction metadata.

It can be any valid :term:`associative array`,
or the equivalent of :term:`null` in your programming language.
The meaning of a "valid associative array" may
depend on the implementation; see the page about
:ref:`implementation-specific deviations <Implementation-Specific Deviations>`.
Here's a JSON example:

.. code-block:: json

   {
      "timestamp": "1510850314",
      "conditions": "blustery and humid",
      "coordinates": {"x": "45", "y": "19"}
   }
