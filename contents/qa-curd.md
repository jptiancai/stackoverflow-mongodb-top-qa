mongodb�����ʹ��"like"����ѯ

����[����](http://stackoverflow.com/questions/3305561/how-to-query-mongodb-with-like)

�����ͱ�������ʹ��:
```
db.users.find({"name": /.*m.*/})
```
�������Ƶ���,
```
db.users.find({"name": /m/})
```

������Ѱ�Ұ���"m"������(SQL��'%'�������ȼ�������ʽ��'.*'),������Ѱ����"m"�̶����ַ�����ʼ������.


�����������ɾ��MongoDB���ݿ�?

����[����](http://stackoverflow.com/questions/8857276/how-do-i-drop-a-mongodb-database-from-the-command-line)

������:

```
mongo <dbname> --eval "db.dropDatabase()"
```

������������нű�shell��Ϣ,�鿴[http://www.mongodb.org/display/DOCS/Scripting+the+shell](http://www.mongodb.org/display/DOCS/Scripting+the+shell)

ʹ��mongoDB shell����г����еļ���?


����[����](http://stackoverflow.com/questions/8866041/how-to-list-all-collections-in-the-mongo-shell)

�����������:

```
db.getCollectionNames()
```

��һ��MongoDB���ݿ���ɾ��һ��

����[����](http://stackoverflow.com/questions/3366397/delete-everything-in-a-mongodb-database)

```
use [database];
db.dropDatabase();
```

mongodb:��λ�ȡ���N����¼?

����[����](http://stackoverflow.com/questions/4421207/mongodb-how-to-get-the-last-n-records)

������˽��������,����Ҫ��������.
�������Ѿ���һЩ��"x"��id���������ֶ�,����������...

#.sort()
-------------


```
db.foo.find().sort({x:1});
```

**1**������������(��ɵ�����˳��),**-1**������(���µ����˳��)
�����ʹ���Զ����ɵ�**_id**�ֶ�,�����������ڵ���Ƕ...���������ʹ����������...
```
db.foo.find().sort({_id:1});
```
������ɵ�����˳�򷵻������ĵ�.

#��Ȼ˳��
-------------

��Ҳ����ʹ�������ᵽ����[��Ȼ˳��](http://docs.mongodb.org/manual/reference/glossary/#term-natural-order)...

```
db.foo.find().sort({$natural:1});
```

�ٴ�����,ʹ��**1**����**-1**ȡ��������Ҫ��˳��.

#ʹ�� .limit()
-------------

���,�������һ�������ʵ�������һ�������ڿ��Ų�ѯ��...

```
db.foo.find().sort({_id:1}).limit(50);
```
����

```
db.foo.find().sort({$natural:1}).limit(50);
```

MongoDB��εȼ�ִ��SQL�е�Join����?

����[����](http://stackoverflow.com/questions/2350495/how-do-i-perform-the-sql-join-equivalent-in-mongodb)

mongodb�ٷ���վ׼ȷ��˵�����������:

[http://docs.mongodb.org/ecosystem/tutorial/model-data-for-ruby-on-rails/](http://docs.mongodb.org/ecosystem/tutorial/model-data-for-ruby-on-rails/)

"����չʾ���ǵĹ����б��ʱ��,��Ҫ˵����������������.�������ʹ�ù�ϵ�����ݿ�,���ǿ������û���͹��±�֮��ʹ�����Ӳ���,��һ�������Ĳ�ѯ�еõ����еĶ���.����MongoDB��֧�����Ӳ���,��ʱ��ҪһЩ���淶������.��������ζ�ſ��Ի���'�û���'����[...]��ϵ���������߿��ܻ�е�������,��ͬ����Υ����һЩ�ձ����.[...]"