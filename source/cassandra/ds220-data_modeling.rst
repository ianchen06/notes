====================
DS220: Data Modeling
====================

.. code-block:: sql
   :caption: To delete all rows in a table

   TRUNCATE table1;

.. code-block:: sql
   :caption: Add or drop columns

   ALTER table1 ADD column type;
   ALTER table1 DROP column;


Collections Column
---------------------

Instead of storing collections data as a string seperated by commas(as you would do in MySQL),
Cassandra provides convenient collection columns

* SET<TEXT>
* LIST<TEXT>
* MAP<TEXT,INT>

User Defined Types
------------------

Since Cassandra does not support JOINs, we need to denormalize some data using UDTs

.. code-block:: sql
   :caption: To add a User Defined Type

   CREATE TYPE video_encoding (
     bit_rates set<text>,
     encoding text,
     height int,
     width int
   );

.. code-block:: sql
   :caption: Create table with UDT

   CREATE TABLE videos (
     video_id UUID,
     name frozen <full_name>,
     students set<frozen <full_name>>,
     addressed map<text, frozen <address>>,
     PRIMARY KEY ((video_id))
   );

.. note::
   UDTs can be nested inside other types, see example above

Other Special Data Types
------------------------

COUNTER

* Can increment, decrement
* Cannot directly assign values to COUNTER

.. note::
   If you have a COUNTER in a table, all other columns must be COUNTER as well, except the PRIMARY KEY columns

.. code-block:: sql

   CREATE TABLE moo_counts (
     cow_name TEXT,
     farm_name TEXT,
     moo_count COUNTER,
     PRIMARY KEY ((cow_name), farm_name) -- cow_name as partition key, farm_name as clustering key
   )

.. code-block:: sql

   UPDATE moo_counts 
   SET moo_count = moo_count + 1
   WHERE cow_name = 'Happy' AND farm_name = 'Home';

Sourcing Files
--------------

.. code-block:: sql
   
   SOURCE './myscript.cql';
