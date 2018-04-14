====
Description
====

Complicated queries sometimes mean building a lot of extremely similar queries. What if it was possible to build parts of a query in a convenient way?

|table|

prepareQuery($sql, $parms = array())
^^^^^^^^^^^^^^
The basic method for building queries.

.. code-block:: php

 $query = $db->prepareQuery('SELECT * FROM users');

prepareWhere($parms = array())
^^^^^^^^^^^^^^
A method used for filtering queries.


.. code-block:: php

 $where = array(new WhereChunk('user_id = ?', array('1'))));
 $query = $db->prepareQuery('SELECT * FROM users');
 $query->prepareWhere($where);

You can use it for simpler queries with defined data and filters.

prepareOrder($orderBy, $sort)
^^^^^^^^^^^^^^
A method used for sorting.

.. code-block:: php

 $where = array(new WhereChunk('active > ?', array('1'))));
 $query = $db->prepareQuery('SELECT * FROM users');
 $query->prepareWhere($where);
 $query->prepareOrder('username', 'ASC');
 
prepareGroupBy($groupBy)
^^^^^^^^^^^^^^^^
Groups the query

.. code-block:: php

 $query = $db->prepareQuery('SELECT * FROM users');
 $query->prepareGroupBy('firstname');

prepareLimit($limit, $offset)
^^^^^^^^^^^^^^
Useful method for limiting appendix data when using the pagination.

.. code-block:: php

 $query = $db->prepareQuery('SELECT * FROM users');
 $query->prepareLimit('10', '30');

prepareParms($parms = array())
^^^^^^^^^^^^^^
Binds the parameter to the query.

.. code-block:: php

 $query = $db->prepareQuery('SELECT * FROM users WHERE id = ?');
 $query->prepareParms(array('1'));

getQuery()
^^^^^^^^^^
Builds the mysql query.

.. code-block:: php

 $query = $db->prepareQuery('SELECT * FROM users');
 $query->prepareOrder('username', 'ASC');
 $prepareQuery = $query->getQuery();

getParams()
^^^^^^^^^^^
Collects the parameters for binding.

.. code-block:: php

 $where = array(new WhereChunk('active > ?', array('1'))));
 $query = $db->prepareQuery('SELECT * FROM users');
 $query->prepareWhere($where);
 $query->prepareOrder('username', 'ASC');
 
 $prepareQuery = $query->getQuery();
 $bindQuery = $query->getParams();

.. |table| advTable:: width="100%"
 :tr_1:
 :th_1.1: Wywołanie
 :th_1.11:
 :th_1.2: Nazwa
 :th_1.22:
 :tr_2:
 :tr_3:
 :td_1.1: MySQL query/-title.1.1
 :td_1.11:
 :td_1.2: prepareQuery()/-title.1.1
 :td_1.22:
 :tr_4:
 :tr_5:
 :td_2.1: bind parms from whereChunk/-title.1.2
 :td_2.11:
 :td_2.2: prepareWhere()/-title.1.2
 :td_2.22:
 :tr_6:
 :tr_8:
 :td_3.1: MySQL order by/-title.1.3
 :td_3.11:
 :td_3.2: prepareOrder()/-title.1.3
 :td_3.22:
 :tr_9:
 :tr_811:
 :td_311.1: MySQL groupBy/-title.1.4
 :td_311.11:
 :td_311.2: prepareGroupBy()/-title.1.4
 :td_311.22:
 :tr_911:
 :tr_10:
 :td_4.1: MySQL limit/-title.1.5
 :td_4.11:
 :td_4.2: prepareLimit()/-title.1.5
 :td_4.22:
 :tr_11:
 :tr_12:
 :td_5.1: MySQL array parms	/-title.1.6
 :td_5.11:
 :td_5.2: prepareParms()/-title.1.6
 :td_5.22:
 :tr_13:
 :tr_14:
 :td_6.1: build MySQL query	/-title.1.7
 :td_6.11:
 :td_6.2: getQuery()/-title.1.7
 :td_6.22:
 :tr_15:
 :tr_16:
 :td_7.1: get MySQL bind parms/-title.1.8
 :td_7.11:
 :td_7.2: getParams()/-title.1.8
 :td_7.22:
 :tr_17:
