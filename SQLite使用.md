##  SQLite使用

####  数据类型

- NULL

- VARCHAR(n)：长度不固定且其最大长度为 n 的字串，n不能超过 4000。

- CHAR(n)：长度固定为n的字串，n不能超过 254。

- INTEGER: 整数

- REAL: 所有值都是浮动的数值,被存储为8字节的IEEE浮动标记序号.

- TEXT: 值为文本字符串,使用数据库编码存储(TUTF-8, UTF-16BE or UTF-16-LE).

- BLOB: 值是BLOB数据块，以输入的数据格式进行存储。如何输入就如何存储,不改 变格式。

- DATA ：包含了 年份、月份、日期。

- TIME： 包含了 小时、分钟、秒。

####  打开或者创建数据库

使用

``` java
openOrCreateDatabase(String path,SQLiteDatabae.CursorFactory factory)
```

打开或者创建一个数据库。它会自动去检测是否存在这个数据库，如果存在则打开，不存在则创建一个数据库；创建成功则返回一个`SQLiteDatabase`对象，否则抛出异常`FileNotFoundException`。

- 参数1 数据库创建的路径

- 参数2 一般设置为null就可以了

```sql
dataBase=SQLiteDatabase.openOrCreateDatabase("/data/data/com.lingdududu.db/databases/stu.db",null);
```


####  创建表

- 编写创建表的SQL语句
- 调用`SQLiteDatabase`的`execSQL()`方法来执行SQL语句

```sql
String table="create table usertable(id int primary key autoincrement,sname varchar(20))"; 

dataBase.execSQL(table); 
```


####  插入数据
插入数据有两种方法：

1. ```java
   insert(String table,String nullColumnHack,ContentValues values)
   ```

   -  参数1 表名称，
   - 参数2 空列的默认值
   -  参数3 `ContentValues`类型的一个封装了列名称和列值的`Map`；

   例：

   ```java
   ContentValues cValue = new ContentValues(); 
   //添加用户名 
   cValue.put("sname","xiaoming"); 
   //添加密码 
   cValue.put("snumber","01005"); 
   //调用insert()方法插入数据 
   dataBase.insert("table",null,cValue);
   ```

2. 编写插入数据的SQL语句，直接调用`execSQL()`方法来执行

####  删除数据

1. ```java
   delete(String table,String whereClause,String[] whereArgs)
   ```

   - 参数1 表名称 
   - 参数2 删除条件(where)
   - 参数3 删除条件值数组

   ```java
   //删除条件 
   String whereClause = "id=?"; 
   //删除条件参数 
   String[] whereArgs = {String.valueOf(2)}; 
   //执行删除 
   dataBase.delete("table",whereClause,whereArgs);
   ```
   
2. 编写删除SQL语句，调用`execSQL()`方法来执行删除。

#####  修改数据

1. ```java
   update(String table,ContentValues values,String whereClause, String[] whereArgs)
   ```

- 参数1 表名称
- 参数2 `ContentValues`类型的键值对
- 参数3 更新条件（where）
- 参数4 更新条件数组

```java
//修改数据
values.put("snumber","101003"); 
//修改条件 
String whereClause = "id=?"; 
//修改添加参数 
String[] whereArgs={String.valuesOf(1)}; 
//修改 
dataBase.update("usertable",values,whereClause,whereArgs);
```

2. 编写更新的SQL语句，`execSQL`执行更新。

#####  查询数据

在Android中查询数据是通过`Cursor`类来实现的，当我们使用`SQLiteDatabase.query()`方法时，会得到一个`Cursor`对象，`Cursor`指向的就是每一条数据。它提供了很多有关查询的方法，具体方法如下：

```java
public Cursor query(String table,
                    //表名称
                    String[] columns,
                    //需要返回的列名称，null则返回全部列
                    String selection,
                    //条件语句，相当于where，可加入？作为参数
                    String[] selectionArgs,
                    //条件语句的参数数组，替换？
                    String groupBy,
                    //查询结果分组条件
                    String having,
                    //groupBy的约束条件
                    String orderBy,
                    //排序
                    String limit);
```

`Cursor`是一个游标接口，提供了遍历查询结果的方法。

`Cursor`游标常用方法

| 方法名称                                   | 方法描述                       |
| ------------------------------------------ | ------------------------------ |
| `getCount()`                               | 获得总的数据项数               |
| `isFirst()`                                | 判断是否第一条记录             |
| `isLast()`                                 | 判断是否最后一条记录           |
| `moveToFirst()`                            | 移动到第一条记录               |
| `moveToLast()`                             | 移动到最后一条记录             |
| `move(int offset)`                         | 移动到指定记录                 |
| `moveToNext()`                             | 移动到下一条记录               |
| `moveToPrevious()`                         | 移动到上一条记录               |
| `getColumnIndexOrThrow(String columnName)` | 根据列名称获得列索引           |
| `getInt(int columnIndex)`                  | 获得指定列索引的`int`类型值    |
| `getString(int columnIndex)`               | 获得指定列索引的`String`类型值 |

####  删除指定表
编写插入数据的SQL语句，直接调用`execSQL()`方法来执行。
