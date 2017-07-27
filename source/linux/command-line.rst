Command-line tricks and tips
============================

Finding files containing string with grep
-----------------------------------------

.. code-block:: bash

   find ./ -type f | xargs grep -P "31\.88"
