.. title:: WhereChunk - Create a database query

.. meta::
    :description: WhereChunk - Helpful in searching/filtering through data in a database
    :keywords: pdo-mysql, query-builder, query
    
WhereChunk
^^^^^^^^^^

So-called chunks. Helpful in searching/filtering through data in a database. When we create a database query, we add various limits WHERE with the help of chunks, we can add or remove parameters added to WHERE more easily and using fewer lines of code.


.. code-block:: php

 namespace Controller;

 use Dframe\Config;
 use Dframe\Database\WhereChunk;
 use Dframe\Database\WhereStringChunk;
 use Dframe\Router\Response;

 class UsersController extends \Controller\Controller
 {    
     /**
      * @return mixed
      */
     public function index()
     {
         $UserModel = $this->loadModel('Users');
         $View = $this->loadView('Index');
         return $View->render('users/index');
     }
         
     /**
      * @return mixed
      */
     public function lists()
     {
         $UserModel = $this->loadModel('Users');
         
         switch ($_SERVER['REQUEST_METHOD']) {
             case 'POST':
                 //Some Method
                 break;

             case 'GET':
                 
                 $limit = $_GET['limit'] ?? '30';
                 $start = $_GET['start'] ?? '0';
                
                 $where = [];
                 $params = [
                     'order' => 'users.user_id', 
                     'sort' => 'ASC'
                 ];

                 if (isset($_GET['search']['username'])) {
                     $where[] = new WhereChunk('`users`.`username`', '%' . $_GET['search']['username'] . '%', 'LIKE');
                 }

                 $users = $UserModel->getUsers($where, $params, $limit, $start);
                     
                 $data = [];
                 foreach ($users['data'] as $key => $user) {
                     $data['id'] = $user['user_id'];
                     $data['first_name'] = $user['user_first_name'];
                     $data['last_name'] = $user['user_last_name'];
                 }
                      
                 return Response::renderJSON(['code' => '200', 'data' => ['users' => ['data' => $data]]], 200);
                 break;
         }

         return Response::renderJSON(['code' => 403, 'message' => 'Method Not Allowed'])
             ->headers(['Allow' => 'GET, POST'])
             ->status(403);
     }
 }

     
.. code-block:: php

 namespace Model;
    
 class UsersModel extends \Model\Model
 {
     /**
      * @param array  $whereObject
      * @param string $order
      * @param string $sort
      *
      * @return array
      */
     public function getUsers($whereObject, $params = ['order' => 'users.id', 'sort' => 'DESC'], $limit = 30, $start = 0)
     {
    
         $query = $this->db->prepareQuery('SELECT * FROM users');
         $query->prepareWhere($whereObject);
         $query->prepareOrder($params['order'], $params['sort']);
         $query->prepareLimit($limit);
         $query->prepareStart($start);
    
         $results = $this->db->pdoQuery($query->getQuery(), $query->getParams())->results();
  
         return $this->methodResult(true, ['data' => $results]);
     }

In case of calling $_POST, a condition is added to the basic query. All parameters are automatically binded to PDO, so we don't have to worry about it anymore.

WhereStringChunk
^^^^^^^^^^^^^^^^

A more interesting class, one that is more often used in practise, is WhereStringChunk - it gives us much more tools than the normal WhereChunk.

.. code-block:: php

 $where = [];
 $where[] = new \Dframe\Database\WhereStringChunk('col_id > ?', ['1']);
 
Or 
 
.. code-block:: php

 $where[] = new \Dframe\Database\WhereStringChunk('col_name LIKE ?', ['%name%']);
 
 
 
HavingStringChunk
^^^^^^^^^^^^^^^^

.. code-block:: php

 $having = [];
 $having[] = new \Dframe\Database\HavingStringChunk('col_name = ?', ['example']);
 
 $query = $this->db->prepareQuery('SELECT * FROM users');
 $query->prepareGroupBy('name');
 $query->prepareHaving($having);
 
