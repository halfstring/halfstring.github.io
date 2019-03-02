---
title: Yii1 +  relations + scopes
date: 2017-03-14 08:00
updated: 2017-03-14 08:00
---


`Yii relations+scopes 过滤结果集`

### 表结构

``` SQL
CREATE TABLE `tbl_project` (
  `id` int(11) NOT NULL,
  `project_title` varchar(100) NOT NULL DEFAULT '',
  `leader_name` varchar(30) NOT NULL COMMENT '项目负责人',
  `create_time` int(11) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='项目表';


INSERT INTO `tbl_project` (`id`, `project_title`, `leader_name`, `create_time`) VALUES
(1, '南京长江二桥十八期维修工程', '张三', 1489502438),
(2, '滨江大道近江路段亮化工程', '李四', 1489202438);
```

```SQL

CREATE TABLE `tbl_task` (
  `id` int(11) NOT NULL,
  `task_name` varchar(100) NOT NULL DEFAULT '',
  `project_id` int(11) NOT NULL DEFAULT '0',
  `status` tinyint(4) NOT NULL DEFAULT '0' COMMENT '0:默认新建；1：正在完成；2：已完成',
  `create_time` int(11) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='任务表';

INSERT INTO `tbl_task` (`id`, `task_name`, `project_id`, `status`, `create_time`) VALUES
(1, '长二工程清洁任务', 1, 1, 1489502438),
(2, '长二工程木工任务', 1, 0, 1489302438),
(3, '滨江碎石任务', 2, 2, 1489102438);

```

``` Model
class Project extends CActiveRecord {

    public static function model($className = __CLASS__) {
        return parent::model($className);
    }

    public function tableName() {
        return '{{project}}';
    }

}
```

```Model
class Task extends CActiveRecord {

    public static function model($className = __CLASS__) {
        return parent::model($className);
    }

    public function tableName() {
        return '{{task}}';
    }

    public function relations() {
        return array(
            'project' => array(
                self::BELONGS_TO,
                'Project',
                'project_id'
            )
        );
    }
}
```

``` php 
$task = Task::model()->findByPk(2);

echo '<pre>';
print_r($task->attributes);
$project = $task->project;
```

结果如下:

```
Array
(
    [id] => 2
    [task_name] => 长二工程木工任务
    [project_id] => 1
    [status] => 0
    [create_time] => 1489302438
)
Array
(
    [id] => 1
    [project_title] => 南京长江二桥十八期维修工程
    [leader_name] => 张三
    [create_time] => 1489502438
)
```
 
 但是某些场景下，project->leader_name的内容可能是敏感数据，在此不想对外展示。
 于是查询结果中 X 掉leader_name 内容；
 
 这里展示另外一种写法来应对这个场景。直接上代码。
 
```php 
 //在Project.php中添如下代码：
 public function scopes() {
    return array(
        'safex' => array(
            'select' => 'id,project_title,create_time',
        ),
    );
}
```

action中修改如下：

``` php
$task = Task::model()->findByPk(2);

echo '<pre>';
print_r($task->attributes);

$project = $task->project('project:safex');

print_r($project->attributes);

```

执行结果如下：

``` php
Array
(
    [id] => 2
    [task_name] => 长二工程木工任务
    [project_id] => 1
    [status] => 0
    [create_time] => 1489302438
)
Array
(
    [id] => 1
    [project_title] => 南京长江二桥十八期维修工程
    [create_time] => 1489502438
    [leader_name] => 
)
```

`leader_name` 对应内容被清空了。

~~Over~~


`打个广告，长期出售果木幼苗，各种规格，源于花木大乡，江苏省邳州市。`
`另，供应专业培育的珍稀花卉：玉玲花(又名：野茉莉)苗木，数量有限, 欲购从速`
`邳州花木之乡`

`请联系：王先生(152 5223 8659)`
