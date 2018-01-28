====
Description
====

The library is a Wrapper, so a class that calls a main class. Its' purpose is simplifying the handling and creating queries for the database, automating a lot of functions. A list of available methods and their descriptions below.

|table|

pdoQuery()
^^^^^^^^^^

Basic methods for most queries

.. code-block:: php

 $result = $db->pdoQuery('SELECT * FROM users WHERE id = ?', array('1'))->result();

Note: It's important to point out that the result() method at the end returns the first row of the table. To receive all rows, you should use results() instead of result()

.. code-block:: php

 $results = $db->pdoQuery('SELECT * FROM users')->results();

select()
^^^^^^^^

A method that's used for collection

.. code-block:: php

 $select = $pdo->select('users')->results();
 
You can use it for simpler queries with defined data and filters

.. code-block:: php

 // Collects all rows
 $select = $pdo->select('users', '*')->results();

.. code-block:: php

 // Collects rows from the given columns
 $select = $pdo->select('users', array('user_id', 'user_name'))->results();

insert()
^^^^^^^^

A method that's use only for adding data to the database

.. code-block:: php

 $dataArray = array('user_name' => 'Jack');
 $insert = $db->insert('users', $dataArray)->getLastInsertId();

InsertBatch()
^^^^^^^^^^^^^

Works on a similar basis as insert(), but it gives us the means to add multiple items

.. code-block:: php

 $dataArray[] = array('user_name' => 'Eli');
 $dataArray[] = array('user_name' => 'Jack');
 $dataArray[] = array('user_name' => 'Mati');
 $insert = $db->insertBatch('users', $dataArray)->getAllLastInsertId();

update()
^^^^^^^^

The most convenient method for updating data in the whole wrapper

.. code-block:: php

 $dataArray = array('user_name'=>'Monana', 'user_age'=> '35');
 $where = array('id' => 23);
 $update = $db->update('user', $dataArray, $aWhere)->affectedRows();

delete()
^^^^^^^^

delete is used for deleting simple data

.. code-block:: php

 $aWhere = array('age' => 35);
 $delete = $db->delete('test', $aWhere)->affectedRows();
In case of deleting more complicated data, related to greater/lesser/similar we use pdoQuery with recommendation of using whereChunkString.

truncate()
^^^^^^^^^^

Clears table

.. code-block:: php

 $truncate = $db->truncate('users');

drop()
^^^^^^

Deletes table

.. code-block:: php

 $drop = $db->drop('users');

describe()
^^^^^^^^^^

Shows a list of columns in the database, along with their types

.. code-block:: php

 $describe = $db->describe('users');

count()
^^^^^^^

Counts the number of rows in the simpler queries

.. code-block:: php

 $count = $db->count('employees');
 $bindWhere = array('user_name' = 'Jack');
 $count = $db->count('users', $bindWhere);

showQuery()
^^^^^^^^^^^

showQuery is a very useful method with big queries: thanks to it, instead of the result()/results() parameter, we use showQuery(), which shows us the Query with the basic variables.

.. code-block:: php

 results = $db->pdoQuery('SELECT * FROM users')->showQuery();
 echo $results;

getLastInsertId()
^^^^^^^^^^^^^^^^^

Returns the last row id added

.. code-block:: php

 $getLastInsertId = $db->insert('users', $dataArray)->getLastInsertId();
 echo $getLastInsertId;

getAllLastInsertId()
^^^^^^^^^^^^^^^^^^^^

Returns a table of all recently added ids for the insertBatch method.

results()
^^^^^^^^^

Returns data in the default array format. Also available xml/json

.. code-block:: php

 $data = $db->results();
 $data = $db->results('xml');
 $data = $db->results('json');

result()
^^^^^^^^

The same principle as results, and, as previously mentioned, returns only the first row.

.. code-block:: php

 $data = $db->result();
 $data = $db->result('xml');
 $data = $db->result('json');

affectedRows()
^^^^^^^^^^^^^^

Returns the number of modified rows

.. code-block:: php

 $data = $db->affectedRows();

start()
^^^^^^^

Start of the msql transaction

.. code-block:: php

 $data = $db->start();

end()
^^^^^

End of the msql transaction

.. code-block:: php

 $data = $db->end();

back()
^^^^^^

REversing the changes in case of error during start/end

.. code-block:: php

 $data = $db->back();

setErrorLog()
^^^^^^^^^^^^^

Set to false by default during the configuration, it turns debug mode on/off

.. code-block:: php

 $db->setErrorLog(true);     // true/false



.. |table| advTable:: width="100%"
 :tr_1:
 :th_1.1: MySQL query/-title.1.1
 :th_1.11:
 :th_1.2: pdoQuery()/-title.1.1
 :th_1.22:
 :tr_2:
 :tr_3:
 :td_1.1: MySQL select query/-title.1.2
 :td_1.11:
 :td_1.2: select()/-title.1.2
 :td_1.22:
 :tr_4:
 :tr_5:
 :td_2.1: MySQL insert query/-title.1.3
 :td_2.11:
 :td_2.2: insert()/-title.1.3
 :td_2.22:
 :tr_6:
 :tr_8:
 :td_3.1: MySQL insert batch/-title.1.4
 :td_3.11:
 :td_3.2: insertBatch()/-title.1.4
 :td_3.22:
 :tr_9:
 :tr_10:
 :td_4.1: MySQL update query/-title.1.5
 :td_4.11:
 :td_4.2: update()/-title.1.5
 :td_4.22:
 :tr_11:
 :tr_12:
 :td_5.1: MySQL delete query/-title.1.6
 :td_5.11:
 :td_5.2: delete()/-title.1.6
 :td_5.22:
 :tr_13:
 :tr_14:
 :td_6.1: MySQL truncate table/-title.1.7
 :td_6.11:
 :td_6.2: truncate()/-title.1.7
 :td_6.22:
 :tr_15:
 :tr_16:
 :td_7.1: MySQL drop table/-title.1.8
 :td_7.11:
 :td_7.2: drop()/-title.1.8
 :td_7.22:
 :tr_17:
 :tr_28:
 :td_8.1: MySQL describe table/-title.1.9
 :td_8.11:
 :td_8.2: describe()/-title.1.9
 :td_8.22:
 :tr_29:
 :tr_30:
 :td_9.1: MySQL count records/-title.1.10
 :td_9.11:
 :td_9.2: count()/-title.1.10
 :td_9.22:
 :tr_31:
 :tr_32:
 :td_10.1: Show/debug executed query/-title.1.11
 :td_10.11:
 :td_10.2: showQuery()/-title.1.11
 :td_10.22:
 :tr_33:
 :tr_34:
 :td_11.1: Get last insert id/-title.1.12
 :td_11.11:
 :td_11.2: getLastInsertId()/-title.1.12
 :td_11.22:
 :tr_35:
 :tr_36:
 :td_12.1: Get all last insert id/-title.1.13
 :td_12.11:
 :td_12.2: getAllLastInsertId()/-title.1.13
 :td_12.22:
 :tr_37:
 :tr_39:
 :td_13.1: Get MySQL results/-title.1.14
 :td_13.11:
 :td_13.2: results()/-title.1.14
 :td_13.22:
 :tr_40:
 :tr_41:
 :td_14.1: Get MySQL result/-title.1.15
 :td_14.11:
 :td_14.2: result()/-title.1.15
 :td_14.22:
 :tr_42:
 :tr_43:
 :td_15.1: Get status of executed query/-title.1.16
 :td_15.11:
 :td_15.2: affectedRows()/-title.1.16
 :td_15.22:
 :tr_44:
 :tr_45:
 :td_16.1: MySQL begin transactions/-title.1.17
 :td_16.11:
 :td_16.2: start()/-title.1.17
 :td_16.22:
 :tr_46:
 :tr_47:
 :td_17.1: MySQL commit the transaction/-title.1.18
 :td_17.11:
 :td_17.2: end()/-title.1.18
 :td_17.22:
 :tr_48:
 :tr_49:
 :td_18.1: MySQL rollback the transaction/-title.1.19
 :td_18.11:
 :td_18.2: back()/-title.1.19
 :td_18.22:
 :tr_50:
 :tr_51:
 :td_19.1: Debugger PDO Error/-title.1.20
 :td_19.11:
 :td_19.2: setErrorLog()/-title.1.20
 :td_19.22:
 :tr_52:
