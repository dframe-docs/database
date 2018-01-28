WhereChunk
^^^^^^^^^^

So-called chunks. Helpful in searching/filtering through data in a database. When we create a database query, we add various limits WHERE with the help of chunks, we can add or remove parameters added to WHERE more easily and using fewer lines of code.

.. code-block:: php

 namespace Controller;
 use Dframe\Config;
 use Dframe\Database\WhereChunk;
 use Dframe\Database\WhereStringChunk;
 use Dframe\Router\Response;
 
 class UserController extends \Controller\Controller
 {
 
     public function index() {
         $userModel = $this->loadModel('User');
         $view = $this->loadView('Index');
         
         switch ($_SERVER['REQUEST_METHOD']) {
             case 'POST':
                 //Some Method
                 break;
                 
             case 'GET':
                 $order = array('user.id', 'ASC');
                 $where = array();
                 
                 if(isset($_GET['search']['username'])){
                     $where[] = new WhereChunk('`users`.`username`', '%'.$_POST['search']['username'].'%', 'LIKE');
                 }
      
                 $users = $userModel->getUsers($where, $order[0], $order[1]);
                 return Response::renderJSON(array('users' => $users), 200);
                 break;
         }
         
         
         return Response::renderJSON(array('code' => 403, 'text' => 'Method Not Allowed'))
             ->headers(array('Allow' => 'GET, POST'))
             ->status(403);
     }
     
.. code-block:: php

 namespace Model;
 
 class UserModel extends \Model\Model
 {
     public function resources($whereObject, $order = 'user.id', $sort = 'DESC'){
 
         $query = $this->baseClass->db->prepareQuery('SELECT * FROM user');        
         $query->prepareWhere($whereObject);
         $query->prepareOrder($order, $sort);
 
         $results = $this->baseClass->db->pdoQuery($query->getQuery(), $query->getParams())->results();
 
         return $this->methodResult(true, array('data' => $results));
     }

In case of calling $_POST, a condition is added to the basic query. All parameters are automatically binded to PDO, so we don't have to worry about it anymore.

WhereStringChunk
^^^^^^^^^^^^^^^^

A more interesting class, one that is more often used in practise, is WhereStringChunk - it gives us much more tools than the normal WhereChunk.

.. code-block:: php

 $where[] = new \Dframe\Database\WhereStringChunk('col_id > ?', array('1'));
