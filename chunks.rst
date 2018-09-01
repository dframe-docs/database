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
             $userModel = $this->loadModel('Users');
             $view = $this->loadView('Index');
             return $view->render('users/index');
         }
         
         /**
          * @return mixed
          */
         public function lists()
         {
             switch ($_SERVER['REQUEST_METHOD']) {
                 case 'POST':
                     //Some Method
                     break;

                 case 'GET':
                     $order = ['users.user_id', 'ASC'];
                     $where = [];

                     if (isset($_GET['search']['username'])) {
                         $where[] = new WhereChunk('`users`.`username`', '%' . $_GET['search']['username'] . '%', 'LIKE');
                     }

                     $users = $userModel->getUsers($where, $order[0], $order[1]);
                     return Response::renderJSON(['code' => '200', 'data' => ['users' => ['data' => $users]]], 200);
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
        public function getUsers($whereObject, $order = 'users.id', $sort = 'DESC')
        {
    
            $query = $this->baseClass->db->prepareQuery('SELECT * FROM users');
            $query->prepareWhere($whereObject);
            $query->prepareOrder($order, $sort);
    
            $results = $this->baseClass->db->pdoQuery($query->getQuery(), $query->getParams())->results();
    
            return $this->methodResult(true, ['data' => $results]);
        }

In case of calling $_POST, a condition is added to the basic query. All parameters are automatically binded to PDO, so we don't have to worry about it anymore.

WhereStringChunk
^^^^^^^^^^^^^^^^

A more interesting class, one that is more often used in practise, is WhereStringChunk - it gives us much more tools than the normal WhereChunk.

.. code-block:: php

 $where[] = new \Dframe\Database\WhereStringChunk('col_id > ?', ['1']);
