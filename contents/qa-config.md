设置MongoDB shell默认美化打印
问题[链接](http://stackoverflow.com/questions/9146123/pretty-print-in-mongodb-shell-as-default)

(注意:这是原始版本的问题的答案,而没有满足“默认”)
你可以这样美化输出:

```
db.collection.find().pretty()
```

> 译者注:第二个答案可以解决默认

你可以增加

```
DBQuery.prototype._prettyShell = true
```
给你的`$HOME/.mongorc.js`文件启用全局默认情况下美化打印.