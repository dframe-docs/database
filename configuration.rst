The data for dframe should be in the app/bootstrap.php file. It contains libraries and variables for the whole application, sent through the variable $this->baseClass. In the web/config.php file, CONST values are added and stored.

.. code-block:: php

 use Dframe\Database\Database;
 
 try {
     $dbConfig = array(
         'host' => DB_HOST,
         'dbname' => DB_DATABASE,
         'username' => DB_USER,
         'password' => DB_PASS
  );
  $db = new Database($dbConfig);
  $db->setErrorLog(false); // Debug
  
 }catch(DBException $e) {
     echo 'The connect can not create: ' . $e->getMessage(); 
     exit();
 }
