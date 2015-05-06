何时使用CouchDB和MongoDB?反之亦然.

问题[链接](http://stackoverflow.com/questions/12437790/when-to-use-couchdb-over-mongodb-and-vice-versa)

略

MongoDB vs. Cassandra

问题[链接](http://stackoverflow.com/questions/2892729/mongodb-vs-cassandra)

略

使用pandas处理“大数据”工作流程


问题[链接](http://stackoverflow.com/questions/14262433/large-data-work-flows-using-pandas)

略

何时使用Redis?何时使用MongoDB?
问题[链接](http://stackoverflow.com/questions/5400163/when-to-redis-when-to-mongodb)

我想说,这个取决于你所在开发团队的类型和你的程序需要.

举例来说,如果有很多查询**需求**,在Redis中意味着更多的工作,你需要使用不同的数据结构适应你的需求.
同样的事情在MongoDB中容易得多.从另一方面说,这是Redis下需要额外的工作才可能偿还**纯粹的速度**.
对于有SQL经验的人来说,学习MongoDB会简单、更小巧.相比之下Redis提供了非传统的方法,因此需要更多的学习,但它有很大的灵活性。

比如.一个**缓存**层可能在Redis中更好实现,至于更模式化的数据,MongoDB可能会更好。__[注意:mongodb是无模式的]__

如果你问我我个人的选择是Redis可以满足大多数需求。

最后,我希望你再看一下[http://oldblog.antirez.com/post/MongoDB-and-Redis.html](http://oldblog.antirez.com/post/MongoDB-and-Redis.html)

NoSQL(MongoDB)与Lucene(或Solr)作为你的数据库

问题[链接](http://stackoverflow.com/questions/3215029/nosql-mongodb-vs-lucene-or-solr-as-your-database)

略

Redis比mongoDB快多少?

问题[链接](http://stackoverflow.com/questions/5252577/how-much-faster-is-redis-than-mongodb)

大概的结果遵从以下指标:**2x 写, 3x 读**
这是一个简单的基准在python中你可以随意调整你的目标,看每个如何执行简单的设置/获取值:

```
#!/usr/bin/env python2.7
import sys, time
from pymongo import Connection
import redis

# connect to redis & mongodb
redis = redis.Redis()
mongo = Connection().test
collection = mongo['test']
collection.ensure_index('key', unique=True)

def mongo_set(data):
    for k, v in data.iteritems():
        collection.insert({'key': k, 'value': v})

def mongo_get(data):
    for k in data.iterkeys():
        val = collection.find_one({'key': k}, fields=('value',)).get('value')

def redis_set(data):
    for k, v in data.iteritems():
        redis.set(k, v)

def redis_get(data):
    for k in data.iterkeys():
        val = redis.get(k)

def do_tests(num, tests):
    # setup dict with key/values to retrieve
    data = {'key' + str(i): 'val' + str(i)*100 for i in range(num)}
    # run tests
    for test in tests:
        start = time.time()
        test(data)
        elapsed = time.time() - start
        print "Completed %s: %d ops in %.2f seconds : %.1f ops/sec" % (test.__name__, num, elapsed, num / elapsed)

if __name__ == '__main__':
    num = 1000 if len(sys.argv) == 1 else int(sys.argv[1])
    tests = [mongo_set, mongo_get, redis_set, redis_get] # order of tests is significant here!
    do_tests(num, tests)
```

结果是在mongodb 1.8.1和Redis2.2.5最新pymongo / redis-py:

```
$ ./cache_benchmark.py 10000
Completed mongo_set: 10000 ops in 1.40 seconds : 7167.6 ops/sec
Completed mongo_get: 10000 ops in 2.38 seconds : 4206.2 ops/sec
Completed redis_set: 10000 ops in 0.78 seconds : 12752.6 ops/sec
Completed redis_get: 10000 ops in 0.89 seconds : 11277.0 ops/sec
```

对结果有保留或怀疑的态度!如果你用另一种语言编程,使用其他客户/不同的实现等你的结果也会不同。更不用说你的使用将是完全不同的!在你打算使用它们时,你最好有自己的基准。作为一个推论,你可能会找出最好的方式使用。总指基准在于你自己.

> 译者注:底下的评论中强调了上述回答是由缺陷的,MongoDB是多线程,而Redis不是,所以你的测试基准如果换成在多核机器上,MongoDB会更高效.