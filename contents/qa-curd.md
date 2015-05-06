mongodb中如何使用"like"来查询

问题[链接](http://stackoverflow.com/questions/3305561/how-to-query-mongodb-with-like)

这样就必须这样使用:
```
db.users.find({"name": /.*m.*/})
```
或者相似的是,
```
db.users.find({"name": /m/})
```

你正在寻找包含"m"的数据(SQL的'%'操作符等价正则表达式的'.*'),而不是寻找以"m"固定在字符串开始的数据.


从命令行如何删除MongoDB数据库?

问题[链接](http://stackoverflow.com/questions/8857276/how-do-i-drop-a-mongodb-database-from-the-command-line)

像这样:

```
mongo <dbname> --eval "db.dropDatabase()"
```

更多关于命令行脚本shell信息,查看[http://www.mongodb.org/display/DOCS/Scripting+the+shell](http://www.mongodb.org/display/DOCS/Scripting+the+shell)

使用mongoDB shell如何列出所有的集合?


问题[链接](http://stackoverflow.com/questions/8866041/how-to-list-all-collections-in-the-mongo-shell)

你可以这样做:

```
db.getCollectionNames()
```

在一个MongoDB数据库中删除一切

问题[链接](http://stackoverflow.com/questions/3366397/delete-everything-in-a-mongodb-database)

```
use [database];
db.dropDatabase();
```

mongodb:如何获取最后N条记录?

问题[链接](http://stackoverflow.com/questions/4421207/mongodb-how-to-get-the-last-n-records)

如果你了解你的问题,你需要升序排列.
假设你已经有一些叫"x"的id或者日期字段,可以这样做...

#.sort()
-------------


```
db.foo.find().sort({x:1});
```

**1**代表升序排列(最旧到最新顺序),**-1**代表降序(最新到最旧顺序)
如果你使用自动生成的**_id**字段,它本事有日期的内嵌...所以你可以使用它来排序...
```
db.foo.find().sort({_id:1});
```
按照最旧到最新顺序返回所有文档.

#自然顺序
-------------

你也可以使用上面提到过的[自然顺序](http://docs.mongodb.org/manual/reference/glossary/#term-natural-order)...

```
db.foo.find().sort({$natural:1});
```

再次重申,使用**1**或者**-1**取决于你想要的顺序.

#使用 .limit()
-------------

最后,你可以做一个不错的实践来添加一个限制在开放查询中...

```
db.foo.find().sort({_id:1}).limit(50);
```
或者

```
db.foo.find().sort({$natural:1}).limit(50);
```

MongoDB如何等价执行SQL中的Join操作?

问题[链接](http://stackoverflow.com/questions/2350495/how-do-i-perform-the-sql-join-equivalent-in-mongodb)

mongodb官方网站准确的说明了这个问题:

[http://docs.mongodb.org/ecosystem/tutorial/model-data-for-ruby-on-rails/](http://docs.mongodb.org/ecosystem/tutorial/model-data-for-ruby-on-rails/)

"我们展示我们的故事列表的时候,需要说明故事中人物姓名.如果我们使用关系型数据库,我们可以在用户表和故事表之间使用连接操作,在一个单独的查询中得到所有的对象.但是MongoDB不支持连接操作,有时需要一些反规范化操作.在这里意味着可以缓存'用户名'属性[...]关系纯粹主义者可能会感到不安了,如同我们违反了一些普遍规律.[...]"