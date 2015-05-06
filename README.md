stackoverflow-mongodb-top-qa
=======================


Notice:

    目前进度90%,有兴趣的同学可以试试, 提pr. or后续我找时间处理完整.

-------------

stackoverflow上MongoDB相关回答整理翻译(相对来说都比较简单/散，非系统学习用途，个人整理而已)

    当前进度: 1/15
    问题个数: 225
    最后更新: 2015-05-06


查看了下前面(vote前15页,挑了下,vote都是100+的样子,大概200个)的问题，[链接](http://stackoverflow.com/questions/tagged/mongodb?page=1&sort=votes&pagesize=15)


如果有兴趣，可以一起翻译

注意，合并了每个问题的多个答案，但是时间仓促，疏漏难免，感兴趣问题直接点链接看原文吧

### 目录
> 基础(增删改查)



> 进阶

 
> 和其他NoSQL比较



#准备阶段

- 系统
```
 # 查看操作系统版本
 head -n 1 /etc/issue
 CentOS release 6.2 (Final)
```

```
#IP:30000为本机器MongoDB的测试数据库地址和端口,可自行配置
mongo 192.168.1.50:30000
MongoDB shell version: 2.4.8
connecting to: 192.168.1.50:30000/test
> use test
switched to db test

```

只检索查询元素在MongoDB集合对象数组中

问题[链接](http://stackoverflow.com/questions/3985214/retrieve-only-the-queried-element-in-an-object-array-in-mongodb-collection)

MongoDB 2.2的新投射操作符'$elemMatch'提供了另一种方式可以更改返回仅包含第一个匹配'shapes'的文档

```
db.test.find(
    {"shapes.color": "red"}, 
    {_id: 0, shapes: {$elemMatch: {color: "red"}}});
```

返回:

```
{"shapes" : [{"shape": "circle", "color": "red"}]}
```

在2.2中,你也可以这样使用'$投射操作符','$'在一个投射对象字段名表明了查询中第一个匹配数组的字段索引.下面返回和上面一样的结果:
```
db.test.find({"shapes.color": "red"}, {_id: 0, 'shapes.$': 1});
```

> 译者注:见MongoDB-manual-2.6 p812页第一项的$elemMatch说明




mongodb中如何更新多个数组元素?

问题[链接](http://stackoverflow.com/questions/4669178/how-to-update-multiple-array-elements-in-mongodb)

现在还不可能用定位参数更新数组中的所有项.查看JIRA[http://jira.mongodb.org/browse/SERVER-1243](http://jira.mongodb.org/browse/SERVER-1243)

作为一个解决方法你可以:
- 单独更新每个项(events.0.handled events.1.handled ...) ...
-  阅读文档,手动编辑和保存较旧的数据(如果你想要更新上的原子性,查看["更新最新数据"](http://docs.mongodb.org/manual/core/write-operations-atomicity/))


MongoDB获得集合中所有键的名称

问题[链接](http://stackoverflow.com/questions/2298870/mongodb-get-names-of-all-keys-in-collection)

你可以使用映射化简:

```
mr = db.runCommand({
  "mapreduce" : "my_collection",
  "map" : function() {
    for (var key in this) { emit(key, null); }
  },
  "reduce" : function(key, stuff) { return null; }, 
  "out": "my_collection" + "_keys"
})
```
然后运行不同的结果集,这样就可以找到所有的键名称:

```
db[mr.result].distinct("_id")
["foo", "bar", "baz", "_id", ...]
```

> 译者注:另外也可参考《MongoDB权威指南》7.3.1 示例:找出集合中的所有键

-----------------------
之前的是访问频率最高的,也很有用!
