��ʱʹ��CouchDB��MongoDB?��֮��Ȼ.

����[����](http://stackoverflow.com/questions/12437790/when-to-use-couchdb-over-mongodb-and-vice-versa)

��

MongoDB vs. Cassandra

����[����](http://stackoverflow.com/questions/2892729/mongodb-vs-cassandra)

��

ʹ��pandas���������ݡ���������


����[����](http://stackoverflow.com/questions/14262433/large-data-work-flows-using-pandas)

��

��ʱʹ��Redis?��ʱʹ��MongoDB?
����[����](http://stackoverflow.com/questions/5400163/when-to-redis-when-to-mongodb)

����˵,���ȡ���������ڿ����Ŷӵ����ͺ���ĳ�����Ҫ.

������˵,����кܶ��ѯ**����**,��Redis����ζ�Ÿ���Ĺ���,����Ҫʹ�ò�ͬ�����ݽṹ��Ӧ�������.
ͬ����������MongoDB�����׵ö�.����һ����˵,����Redis����Ҫ����Ĺ����ſ��ܳ���**������ٶ�**.
������SQL���������˵,ѧϰMongoDB��򵥡���С��.���֮��Redis�ṩ�˷Ǵ�ͳ�ķ���,�����Ҫ�����ѧϰ,�����кܴ������ԡ�

����.һ��**����**�������Redis�и���ʵ��,���ڸ�ģʽ��������,MongoDB���ܻ���á�__[ע��:mongodb����ģʽ��]__

����������Ҹ��˵�ѡ����Redis����������������

���,��ϣ�����ٿ�һ��[http://oldblog.antirez.com/post/MongoDB-and-Redis.html](http://oldblog.antirez.com/post/MongoDB-and-Redis.html)

NoSQL(MongoDB)��Lucene(��Solr)��Ϊ������ݿ�

����[����](http://stackoverflow.com/questions/3215029/nosql-mongodb-vs-lucene-or-solr-as-your-database)

��

Redis��mongoDB�����?

����[����](http://stackoverflow.com/questions/5252577/how-much-faster-is-redis-than-mongodb)

��ŵĽ���������ָ��:**2x д, 3x ��**
����һ���򵥵Ļ�׼��python�����������������Ŀ��,��ÿ�����ִ�м򵥵�����/��ȡֵ:

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

�������mongodb 1.8.1��Redis2.2.5����pymongo / redis-py:

```
$ ./cache_benchmark.py 10000
Completed mongo_set: 10000 ops in 1.40 seconds : 7167.6 ops/sec
Completed mongo_get: 10000 ops in 2.38 seconds : 4206.2 ops/sec
Completed redis_set: 10000 ops in 0.78 seconds : 12752.6 ops/sec
Completed redis_get: 10000 ops in 0.89 seconds : 11277.0 ops/sec
```

�Խ���б������ɵ�̬��!���������һ�����Ա��,ʹ�������ͻ�/��ͬ��ʵ�ֵ���Ľ��Ҳ�᲻ͬ��������˵���ʹ�ý�����ȫ��ͬ��!�������ʹ������ʱ,��������Լ��Ļ�׼����Ϊһ������,����ܻ��ҳ���õķ�ʽʹ�á���ָ��׼�������Լ�.

> ����ע:���µ�������ǿ���������ش�����ȱ�ݵ�,MongoDB�Ƕ��߳�,��Redis����,������Ĳ��Ի�׼��������ڶ�˻�����,MongoDB�����Ч.