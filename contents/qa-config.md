����MongoDB shellĬ��������ӡ
����[����](http://stackoverflow.com/questions/9146123/pretty-print-in-mongodb-shell-as-default)

(ע��:����ԭʼ�汾������Ĵ�,��û�����㡰Ĭ�ϡ�)
����������������:

```
db.collection.find().pretty()
```

> ����ע:�ڶ����𰸿��Խ��Ĭ��

���������

```
DBQuery.prototype._prettyShell = true
```
�����`$HOME/.mongorc.js`�ļ�����ȫ��Ĭ�������������ӡ.