---
title: Cakephp Validation Tips
date: 2021-10-30 23:44:21
tags:
- cakephp
clearReading: true
coverCaption: "Hello World, Hello Programming"
coverSize: partial
comments: false
categories: Back-end Knowledge
---
Cakephp Validation踩的坑
<!--more-->
最近在捣鼓cakephp(非自愿),踩过的一些小坑在这里记录一下。

### 1. 关于Cakephp的常用配置约定
Cakephp是MVC框架。它的命名遵循一定的配置约定。大致的代码都保存在`src`目录下。

#### 控制器(Controller)约定
控制器的类名是复数，首字母大写, 并且以 Controller 结尾。
UsersController 和 ArticleCategoriesController 都是约定的控制器类名的例子。
控制器中 public 的方法通常被暴露为可通过web浏览器访问的“操作”。

例：
 `/users/view` 映射到`UsersController`中的 view() 方法。

`/article-categories/view-all` 是访问 ArticleCategoriesController::viewAll() 方法。

#### 模型(Model)约定
CakePHP 的模型是由 `Table` and `Entity` 两种对象组成。
Table 为是一个特定的数据库表 的抽象。他们储存在 `src/Model/Table` 目录中。首先我们建立文件 `src/Model/Table/ArticlesTable.php`。

`Table` 类名是复数、首字母大写、以 Table 结尾的。UsersTable、ArticleCategoriesTable、UserFavoritePagesTable 分别是对应 users、article_categories、user_favorite_pages 表的 table 类名。

`Entity` 类名是单数、首字母大写、无后缀的。User、ArticleCategory、UserFavoritePage 分别是对应 users、article_categories、user_favorite_pages 表的 entity 类名。

#### 视图(View)约定
视图模板文件使用它对应的控制器方法的名字以下划线形式命名。
模版文件都储存在 `src/Template` 目录中的一个文件夹中。此文件夹以其对应的控制器命名。
`ArticlesController`类的 viewAll() 防范将会对应视图模板文件 `src/Template/Articles/view_all.ctp`。

总结一下就是下面这样的，遵行约定可以减少我们进行配置的时间，从而提高开发效率。
```
数据库表：articles

Table 类：ArticlesTable，在文件 src/Model/Table/ArticlesTable.php 中

Entity 类：Article，在文件 src/Model/Entity/Article.php 中

控制器类：ArticlesController，在文件 src/Controller/ArticlesController.php 中

视图模板，在文件 src/Template/Articles/index.ctp 中
```

### 2.关于Validation的设置
Cakephp的Validation设置和验证都可以写在相应的Model Table文件里。
当 CakePHP 调用 `save()` 时，validationDefault() 方法将指示如何验证数据。在下面的代码例子中， 我们规定 `title` 和 `body` 不可以为空，而且必须要达到一定的长度。

例:
```php
// src/Model/Table/ArticlesTable.php

// add this use statement right below the namespace declaration to import
// the Validator class
use Cake\Validation\Validator;

// Add the following method.
public function validationDefault(Validator $validator)
{
    $validator
        ->notEmpty('title')
        ->minLength('title', 10)
        ->maxLength('title', 255)

        ->notEmpty('body')
        ->minLength('body', 10);

    return $validator;
}
```

当然你也可以自己定义Validation文件来满足你的开发需要。
比如可以在`Model`文件夹下建立一个`Validation`文件夹，然后在里面继承Validation方法并进行自定义。

例：
Path: Model/Validation/ClientValidation

```php
<?php

namespace Airticle\Model\Validation;

use Cake\ORM\TableRegistry;
use Cake\Validation\Validation;
use Cake\Validation\Validator;

class ClientValidation extends Validator
{
    public function airticleValidation($data) {
        if (isset($data['AIRTICLE'])) {
            $this->clientValidationAirticle();
        }
    }

    public function clientValidationAirticle($value, $data) {
        // 这里的两个参数一个为改项目的值, 另一个为controller相关的所有date
        $this
            ->notEmpty('AIRTICLE', '请输入文章名字')
            ->add('AIRTICLE', 'length', ['rule' => ['maxLength', '25'], 'message' => '文章名字太长了！'])
    }
}
```

当我们想要使用自定义的Validation的时候可以在相应的Controller里进行呼出。

然后在Controller里的方法里进行使用:

例：
Path: Controller/AirticleController.php

```php
// 首先需要在Controller里先申明`use`
use Airticle\Model\Validation\ClientValidation;
...

// 进行Validation方法的自定义
$this->dataValid();

...

// 在这里具体呼出Model里定义好的Validation, 该方法为private.
private function dataValid() {
    // 先new一下
    $validator = new ClientValidation();

    // 定义一个用于储存error结果的数列
    $errors = [];

    // 这里是将文章的数据传给Model里的Validation进行判断，并将返回的error结果代入储存error结果的数列。
    $errors[] = $validator->airticleValidation($this->request->data('data.airticle_data'));
}
```

这样我们就可以完成Validation的自定义了，当我们在不同的功能页面里需要使用同样的Validation判断的时候可以考虑采用这样的方法。

当然Cakephp还提供了很多智能的Validation方法，比如邮件地址的判断, 电话号码的判断等，我们可以直接使用他们或者也可以自定义相关项目的Validation.

官方文档参考：
[Validation](https://book.cakephp.org/3/en/core-libraries/validation.html)






