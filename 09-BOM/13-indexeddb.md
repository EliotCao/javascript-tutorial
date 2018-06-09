# IndexedDB API

## 概述

随着浏览器的功能不断增强，越来越多的网站开始考虑，将大量数据储存在客户端，这样可以减少从服务器获取数据，直接从本地获取数据。

现有的浏览器数据储存方案，都不适合储存大量数据：Cookie 的大小不超过 4KB，且每次请求都会发送回服务器；LocalStorage 在 2.5MB 到 10MB 之间（各家浏览器不同），而且不提供搜索功能，不能建立自定义的索引。所以，需要一种新的解决方案，这就是 IndexedDB 诞生的背景。

通俗地说，IndexedDB 就是浏览器提供的本地数据库，它可以被网页脚本创建和操作。IndexedDB 允许储存大量数据，提供查找接口，还能建立索引。这些都是 LocalStorage 所不具备的。就数据库类型而言，IndexedDB 不属于关系型数据库（不支持 SQL 查询语句），更接近 NoSQL 数据库。

IndexedDB 具有以下特点。

**（1）键值对储存。** IndexedDB 内部采用对象仓库（object store）存放数据。所有类型的数据都可以直接存入，包括 JavaScript 对象。对象仓库中，数据以“键值对”的形式保存，每一个数据记录都有对应的主键，主键是独一无二的，不能有重复，否则会抛出一个错误。

**（2）异步。** IndexedDB 操作时不会锁死浏览器，用户依然可以进行其他操作，这与 LocalStorage 形成对比，后者的操作是同步的。异步设计是为了防止大量数据的读写，拖慢网页的表现。

**（3）支持事务。** IndexedDB 支持事务（transaction），这意味着一系列操作步骤之中，只要有一步失败，整个事务就都取消，数据库回滚到事务发生之前的状态，不存在只改写一部分数据的情况。

**（4）同源限制。** IndexedDB 受到同源限制，每一个数据库对应创建它的域名。网页只能访问自身域名下的数据库，而不能访问跨域的数据库。

**（5）储存空间大。** IndexedDB 的储存空间比 LocalStorage 大得多，一般来说不少于 250MB，甚至没有上限。

**（6）支持二进制储存。** IndexedDB 不仅可以储存字符串，还可以储存二进制数据（ArrayBuffer 对象和 Blob 对象）。

## 基本概念

IndexedDB 是一个比较复杂的 API，涉及不少概念。它把不同的实体，抽象成一个个对象接口。学习这个 API，就是学习它的各种对象接口。

- 数据库：IDBDatabase 对象
- 对象仓库：IDBObjectStore 对象
- 索引： IDBIndex 对象
- 事务： IDBTransaction 对象
- 操作请求：IDBRequest 对象
- 指针： IDBCursor 对象
- 主键集合：IDBKeyRange 对象

下面是一些主要的概念。

**（1）数据库**

数据库是一系列相关数据的容器。每个域名（严格的说，是协议 + 域名 + 端口）都可以新建任意多个数据库。

IndexedDB 数据库有版本的概念。同一个时刻，只能有一个版本的数据库存在。如果要修改数据库结构（新增或删除表、索引或者主键），只能通过升级数据库版本完成。

**（2）对象仓库**

每个数据库包含若干个对象仓库（object store）。它类似于关系型数据库的表格。

**（3）数据记录**

对象仓库保存的是数据记录。每条记录类似于关系型数据库的行，但是只有主键和数据体两部分。主键用来建立默认的索引，必须是不同的，否则会报错。主键可以是数据记录里面的一个属性，也可以指定为一个递增的整数编号。

```
{ id: 1, text: 'foo' }
```

上面的对象中，`id`属性可以当作主键。

数据体可以是任意数据类型，不限于对象。

**（4）索引**

为了加速数据的检索，可以在对象仓库里面，为不同的属性建立索引。

**（5）事务**

数据记录的读写和删改，都要通过事务完成。事务对象提供`error`、`abort`和`complete`三个事件，用来监听操作结果。

## 操作流程

IndexedDB 数据库的各种操作，一般是按照下面的流程进行的。这个部分只给出简单的代码示例，用于快速上手，详细的各个对象的 API 放在后文介绍。

### 打开数据库

使用 IndexedDB 的第一步是打开数据库，使用`indexedDB.open()`方法。

```
var request = window.indexedDB.open(databaseName, version);
```

这个方法接受两个参数，第一个参数是字符串，表示数据库的名字。如果指定的数据库不存在，就会新建数据库。第二个参数是整数，表示数据库的版本。如果省略，打开已有数据库时，默认为当前版本；新建数据库时，默认为`1`。

`indexedDB.open()`方法返回一个 IDBRequest 对象。这个对象通过三种事件`error`、`success`、`upgradeneeded`，处理打开数据库的操作结果。

**（1）error 事件**

`error`事件表示打开数据库失败。

```
request.onerror = function (event) {
  console.log('数据库打开报错');
};
```

**（2）success 事件**

`success`事件表示成功打开数据库。

```
var db;

request.onsuccess = function (event) {
  db = request.result;
  console.log('数据库打开成功');
};
```

这时，通过`request`对象的`result`属性拿到数据库对象。

**（3）upgradeneeded 事件**

如果指定的版本号，大于数据库的实际版本号，就会发生数据库升级事件`upgradeneeded`。

```
var db;

request.onupgradeneeded = function (event) {
  db = event.target.result;
}
```

这时通过事件对象的`target.result`属性，拿到数据库实例。

### 新建数据库

新建数据库与打开数据库是同一个操作。如果指定的数据库不存在，就会新建。不同之处在于，后续的操作主要在`upgradeneeded`事件的监听函数里面完成，因为这时版本从无到有，所以会触发这个事件。

通常，新建数据库以后，第一件事是新建对象仓库（即新建表）。

```
request.onupgradeneeded = function(event) {
  db = event.target.result;
  var objectStore = db.createObjectStore('person', { keyPath: 'id' });
}
```

上面代码中，数据库新建成功以后，新增一张叫做`person`的表格，主键是`id`。

更好的写法是先判断一下，这张表格是否存在，如果不存在再新建。

```
request.onupgradeneeded = function (event) {
  db = event.target.result;
  var objectStore;
  if (!db.objectStoreNames.contains('person')) {
    objectStore = db.createObjectStore('person', { keyPath: 'id' });
  }
}
```

主键（key）是默认建立索引的属性。比如，数据记录是`{ id: 1, name: '张三' }`，那么`id`属性可以作为主键。主键也可以指定为下一层对象的属性，比如`{ foo: { bar: 'baz' } }`的`foo.bar`也可以指定为主键。

如果数据记录里面没有合适作为主键的属性，那么可以让 IndexedDB 自动生成主键。

```
var objectStore = db.createObjectStore(
  'person',
  { autoIncrement: true }
);
```

上面代码中，指定主键为一个递增的整数。

新建对象仓库以后，下一步可以新建索引。

```
request.onupgradeneeded = function(event) {
  db = event.target.result;
  var objectStore = db.createObjectStore('person', { keyPath: 'id' });
  objectStore.createIndex('name', 'name', { unique: false });
  objectStore.createIndex('email', 'email', { unique: true });
}
```

上面代码中，`IDBObject.createIndex()`的三个参数分别为索引名称、索引所在的属性、配置对象（说明该属性是否包含重复的值）。