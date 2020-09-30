---
title: Create A Temple Page With Docker Bake
date: 2019-04-06 20:00:52
tags:
- Docker
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Back-end Knowledge
---

Docker环境下使用bake自动生成页面
<!--more-->

### 上节回顾
使用Docker配置Cakephp3开发环境

上节做到了用Docker构建环境并连接好了数据库，这节使用bake进行自动化页面的生成。

***

### 执行bake

执行bake时要注意，执行commend的场所在bin路径下。执行前确保数据库里存在相应的表。

1.创建User表

```
// User Table
CREATE TABLE `users` (
    `id` int(11) unsigned NOT NULL AUTO_INCREMENT PRIMARY KEY,
    `email` varchar(255) NOT NULL,
    `password` varchar(255) NOT NULL,
    `status` char(1) DEFAULT 0,
    `created` DATETIME DEFAULT NULL,
    `modified` DATETIME DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;
```

2.使用bake进行users全页面自动生成

```
# bin/cake bake all users
Bake All
---------------------------------------------------------------
One moment while associations are detected.
 
Baking table class for Users...
 
Creating file /var/www/html/cakephp3.com/src/Model/Table/UsersTable.php
Wrote `/var/www/html/cakephp3.com/src/Model/Table/UsersTable.php`
Deleted `/var/www/html/cakephp3.com/src/Model/Table/empty`
 
Baking entity class for User...
 
Creating file /var/www/html/cakephp3.com/src/Model/Entity/User.php
Wrote `/var/www/html/cakephp3.com/src/Model/Entity/User.php`
Deleted `/var/www/html/cakephp3.com/src/Model/Entity/empty`
 
Baking test fixture for Users...
 
Creating file /var/www/html/cakephp3.com/tests/Fixture/UsersFixture.php
Wrote `/var/www/html/cakephp3.com/tests/Fixture/UsersFixture.php`
Deleted `/var/www/html/cakephp3.com/tests/Fixture/empty`
Bake is detecting possible fixtures...
 
Baking test case for App\Model\Table\UsersTable ...
 
Creating file /var/www/html/cakephp3.com/tests/TestCase/Model/Table/UsersTableTest.php
Wrote `/var/www/html/cakephp3.com/tests/TestCase/Model/Table/UsersTableTest.php`
 
Baking controller class for Users...
 
Creating file /var/www/html/cakephp3.com/src/Controller/UsersController.php
Wrote `/var/www/html/cakephp3.com/src/Controller/UsersController.php`
Bake is detecting possible fixtures...
 
Baking test case for App\Controller\UsersController ...
 
Creating file /var/www/html/cakephp3.com/tests/TestCase/Controller/UsersControllerTest.php
Wrote `/var/www/html/cakephp3.com/tests/TestCase/Controller/UsersControllerTest.php`
 
Baking `index` view template file...
 
Creating file /var/www/html/cakephp3.com/src/Template/Users/index.ctp
Wrote `/var/www/html/cakephp3.com/src/Template/Users/index.ctp`
 
Baking `view` view template file...
 
Creating file /var/www/html/cakephp3.com/src/Template/Users/view.ctp
Wrote `/var/www/html/cakephp3.com/src/Template/Users/view.ctp`
 
Baking `add` view template file...
 
Creating file /var/www/html/cakephp3.com/src/Template/Users/add.ctp
Wrote `/var/www/html/cakephp3.com/src/Template/Users/add.ctp`
 
Baking `edit` view template file...
 
Creating file /var/www/html/cakephp3.com/src/Template/Users/edit.ctp
Wrote `/var/www/html/cakephp3.com/src/Template/Users/edit.ctp`
Bake All complete.
```

3.bake自动生成的页面确认

此时user表为基准，生成了一系列users信息表（预览，添加，修改，删除）
user添加页面： https://开发环境的URL/users/add
<br>
<img src="./1.png" style="width:500px">

***

### 自动生成的文件

1.Model/Table/UsersTable.php

```PHP
# cat src/Model/Table/UsersTable.php
<?php
namespace App\Model\Table;
 
use Cake\ORM\Query;
use Cake\ORM\RulesChecker;
use Cake\ORM\Table;
use Cake\Validation\Validator;
 
/**
 * Users Model
 *
 * @method \App\Model\Entity\User get($primaryKey, $options = [])
 * @method \App\Model\Entity\User newEntity($data = null, array $options = [])
 * @method \App\Model\Entity\User[] newEntities(array $data, array $options = [])
 * @method \App\Model\Entity\User|bool save(\Cake\Datasource\EntityInterface $entity, $options = [])
 * @method \App\Model\Entity\User patchEntity(\Cake\Datasource\EntityInterface $entity, array $data, array $options = [])
 * @method \App\Model\Entity\User[] patchEntities($entities, array $data, array $options = [])
 * @method \App\Model\Entity\User findOrCreate($search, callable $callback = null, $options = [])
 *
 * @mixin \Cake\ORM\Behavior\TimestampBehavior
 */
class UsersTable extends Table
{
 
    /**
     * Initialize method
     *
     * @param array $config The configuration for the Table.
     * @return void
     */
    public function initialize(array $config)
    {
        parent::initialize($config);
 
        $this->setTable('users');
        $this->setDisplayField('id');
        $this->setPrimaryKey('id');
 
        $this->addBehavior('Timestamp');
    }
 
    /**
     * Default validation rules.
     *
     * @param \Cake\Validation\Validator $validator Validator instance.
     * @return \Cake\Validation\Validator
     */
    public function validationDefault(Validator $validator)
    {
        $validator
            ->integer('id')
            ->allowEmpty('id', 'create');
 
        $validator
            ->email('email')
            ->requirePresence('email', 'create')
            ->notEmpty('email');
 
        $validator
            ->scalar('password')
            ->requirePresence('password', 'create')
            ->notEmpty('password');
 
        $validator
            ->scalar('status')
            ->allowEmpty('status');
 
        return $validator;
    }
 
    /**
     * Returns a rules checker object that will be used for validating
     * application integrity.
     *
     * @param \Cake\ORM\RulesChecker $rules The rules object to be modified.
     * @return \Cake\ORM\RulesChecker
     */
    public function buildRules(RulesChecker $rules)
    {
        $rules->add($rules->isUnique(['email']));
 
        return $rules;
    }
}
```

***

2.Model/Entity/User.php

```PHP
# cat src/Model/Entity/User.php 
<?php
namespace App\Model\Entity;
 
use Cake\ORM\Entity;
 
/**
 * User Entity
 *
 * @property int $id
 * @property string $email
 * @property string $password
 * @property string $status
 * @property \Cake\I18n\FrozenTime $created
 * @property \Cake\I18n\FrozenTime $modified
 */
class User extends Entity
{
 
    /**
     * Fields that can be mass assigned using newEntity() or patchEntity().
     *
     * Note that when '*' is set to true, this allows all unspecified fields to
     * be mass assigned. For security purposes, it is advised to set '*' to false
     * (or remove it), and explicitly make individual fields accessible as needed.
     *
     * @var array
     */
    protected $_accessible = [
        'email' => true,
        'password' => true,
        'status' => true,
        'created' => true,
        'modified' => true
    ];
 
    /**
     * Fields that are excluded from JSON versions of the entity.
     *
     * @var array
     */
    protected $_hidden = [
        'password'
    ];
}
```

***

3.Template/Users/index.ctp

```PHP
# cat src/Template/Users/index.ctp 
<?php
/**
 * @var \App\View\AppView $this
 * @var \App\Model\Entity\User[]|\Cake\Collection\CollectionInterface $users
 */
?>
<nav class="large-3 medium-4 columns" id="actions-sidebar">
    <ul class="side-nav">
        <li class="heading"><?= __('Actions') ?></li>
        <li><?= $this->Html->link(__('New User'), ['action' => 'add']) ?></li>
    </ul>
</nav>
<div class="users index large-9 medium-8 columns content">
    <h3><?= __('Users') ?></h3>
    <table cellpadding="0" cellspacing="0">
        <thead>
            <tr>
                <th scope="col"><?= $this->Paginator->sort('id') ?></th>
                <th scope="col"><?= $this->Paginator->sort('email') ?></th>
                <th scope="col"><?= $this->Paginator->sort('password') ?></th>
                <th scope="col"><?= $this->Paginator->sort('status') ?></th>
                <th scope="col"><?= $this->Paginator->sort('created') ?></th>
                <th scope="col"><?= $this->Paginator->sort('modified') ?></th>
                <th scope="col" class="actions"><?= __('Actions') ?></th>
            </tr>
        </thead>
        <tbody>
            <?php foreach ($users as $user): ?>
            <tr>
                <td><?= $this->Number->format($user->id) ?></td>
                <td><?= h($user->email) ?></td>
                <td><?= h($user->password) ?></td>
                <td><?= h($user->status) ?></td>
                <td><?= h($user->created) ?></td>
                <td><?= h($user->modified) ?></td>
                <td class="actions">
                    <?= $this->Html->link(__('View'), ['action' => 'view', $user->id]) ?>
                    <?= $this->Html->link(__('Edit'), ['action' => 'edit', $user->id]) ?>
                    <?= $this->Form->postLink(__('Delete'), ['action' => 'delete', $user->id], ['confirm' => __('Are you sure you want to delete # {0}?', $user->id)]) ?>
                </td>
            </tr>
            <?php endforeach; ?>
        </tbody>
    </table>
    <div class="paginator">
        <ul class="pagination">
            <?= $this->Paginator->first('<< ' . __('first')) ?>
            <?= $this->Paginator->prev('< ' . __('previous')) ?>
            <?= $this->Paginator->numbers() ?>
            <?= $this->Paginator->next(__('next') . ' >') ?>
            <?= $this->Paginator->last(__('last') . ' >>') ?>
        </ul>
        <p><?= $this->Paginator->counter(['format' => __('Page {{page}} of {{pages}}, showing {{current}} record(s) out of {{count}} total')]) ?></p>
    </div>
</div>
```

***

4.Controller/UsersController.php

```PHP
# cat src/Controller/UsersController.php 
<?php
namespace App\Controller;
 
use App\Controller\AppController;
 
/**
 * Users Controller
 *
 * @property \App\Model\Table\UsersTable $Users
 *
 * @method \App\Model\Entity\User[] paginate($object = null, array $settings = [])
 */
class UsersController extends AppController
{
 
    /**
     * Index method
     *
     * @return \Cake\Http\Response|void
     */
    public function index()
    {
        $users = $this->paginate($this->Users);
 
        $this->set(compact('users'));
        $this->set('_serialize', ['users']);
    }
 
    /**
     * View method
     *
     * @param string|null $id User id.
     * @return \Cake\Http\Response|void
     * @throws \Cake\Datasource\Exception\RecordNotFoundException When record not found.
     */
    public function view($id = null)
    {
        $user = $this->Users->get($id, [
            'contain' => []
        ]);
 
        $this->set('user', $user);
        $this->set('_serialize', ['user']);
    }
 
    /**
     * Add method
     *
     * @return \Cake\Http\Response|null Redirects on successful add, renders view otherwise.
     */
    public function add()
    {
        $user = $this->Users->newEntity();
        if ($this->request->is('post')) {
            $user = $this->Users->patchEntity($user, $this->request->getData());
            if ($this->Users->save($user)) {
                $this->Flash->success(__('The user has been saved.'));
 
                return $this->redirect(['action' => 'index']);
            }
            $this->Flash->error(__('The user could not be saved. Please, try again.'));
        }
        $this->set(compact('user'));
        $this->set('_serialize', ['user']);
    }
 
    /**
     * Edit method
     *
     * @param string|null $id User id.
     * @return \Cake\Http\Response|null Redirects on successful edit, renders view otherwise.
     * @throws \Cake\Network\Exception\NotFoundException When record not found.
     */
    public function edit($id = null)
    {
        $user = $this->Users->get($id, [
            'contain' => []
        ]);
        if ($this->request->is(['patch', 'post', 'put'])) {
            $user = $this->Users->patchEntity($user, $this->request->getData());
            if ($this->Users->save($user)) {
                $this->Flash->success(__('The user has been saved.'));
 
                return $this->redirect(['action' => 'index']);
            }
            $this->Flash->error(__('The user could not be saved. Please, try again.'));
        }
        $this->set(compact('user'));
        $this->set('_serialize', ['user']);
    }
 
    /**
     * Delete method
     *
     * @param string|null $id User id.
     * @return \Cake\Http\Response|null Redirects to index.
     * @throws \Cake\Datasource\Exception\RecordNotFoundException When record not found.
     */
    public function delete($id = null)
    {
        $this->request->allowMethod(['post', 'delete']);
        $user = $this->Users->get($id);
        if ($this->Users->delete($user)) {
            $this->Flash->success(__('The user has been deleted.'));
        } else {
            $this->Flash->error(__('The user could not be deleted. Please, try again.'));
        }
 
        return $this->redirect(['action' => 'index']);
    }
}
```