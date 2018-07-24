.. title:: Configuration - PDO wrapper

.. meta::
    :description: The data for dframe should be in the app/bootstrap.php file. It contains libraries and variables for the whole application, sent through the variable $this->baseClass.
    :keywords: dframe, database, pdo, pdo-mysql, query-builder, query

    
====
Configuration
====

The data for dframe should be in the app/bootstrap.php file. It contains libraries and variables for the whole application, sent through the variable $this->baseClass. In the web/config.php file, CONST values are added and stored.

.. code-block:: php

 use Dframe\Database\Database;
 use \PDO;
 
 try {
    $dbConfig = [
        'host' => DB_HOST,
        'dbname' => DB_DATABASE,
        'username' => DB_USER,
        'password' => DB_PASS
    ];
    
    // Debug Config 
    $config = [
        'logDir' => APP_DIR . 'View/logs/',
        'attributes' => [
            PDO::MYSQL_ATTR_INIT_COMMAND => "SET NAMES utf8", 
            //PDO::ATTR_ERRMODE => PDO::ERRMODE_SILENT,  // Set pdo error mode silent
            PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION, // If you want to Show Class exceptions on Screen, Uncomment below code 
            PDO::ATTR_EMULATE_PREPARES => true, // Use this setting to force PDO to either always emulate prepared statements (if TRUE), or to try to use native prepared statements (if FALSE). 
            PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC // Set default pdo fetch mode as fetch assoc
         ]
    ];
    
    $db = new Database($dbConfig, $config);
    $db->setErrorLog(false); // Debug
  
 }catch(DBException $e) {
     echo 'The connect can not create: ' . $e->getMessage(); 
     exit();
 }
