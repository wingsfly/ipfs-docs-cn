---
title: 不变性
sidebarDepth: 0
issueUrl: https://github.com/ipfs/docs/issues/386
description: Learn about the concept of data immutability and how it's critical to how IPFS works.
related:
  'IPFS Docs: Content addressing and CIDs': /concepts/content-addressing/
  'Video: How IPFS deals with files (IPFS Camp 2019)': https://www.youtube.com/watch?v=Z5zNPwMDYGg
  'Tutorial: How IPFS deals with files (IPFS Camp 2019)': https://github.com/ipfs/camp/tree/master/CORE_AND_ELECTIVE_COURSES/CORE_COURSE_A
  'Wikipedia: Content-addressable storage': https://en.wikipedia.org/wiki/Content-addressable_storage
---

# 不变性

一个不可变对象是指其状态一旦创建就不可变更或修改的对象。一旦一个文件添加到IPFS网络中，在不改变其[内容标识符(CID)](/concepts/content-addressing)的前提下，该文件的内容就无法修改。该特性非常适合存储无需修改的数据，然而当需要修改或更新其内容时，不可变性就成为了一个问题。本页就是讨论如何从不可变的构建块，来构建一个持续可变状态。

CID是指向内容的绝对指针，任何时候请求一个CID，其对应的内容值都会一样的。这是内容机制的一部分，不可能被更改。为了在一个可变系统中管理不可变的文件，我们需要在CID之上添加一个新的功能层。

作为一个基础示例，我们现在有两块内容，分别为字符串`hello`和`world`，他们作为两个叶子节点，其CID分别为`A`和`B`。如果我们把这两个节点连接起来，将获得一个新的CID`C`。在根CID之上，我们分配了一个指针`Pointer`。

```
   +-----------+
   |  Pointer  |
   +-----------+
         ↓
      +-----+
   +--|  C  |-+
   |  +-----+ |
   |          |
+-----+    +-----+
|  A  |    |  B  |
+-----+    +-----+
"hello"    "world"
```

如果我们把`B`的内容修改为`IPFS!`，所有其上游的路径都会发生改变。在这个示例中，上游路径中只有`C`。如果我们从指针那里请求内容，我们获得了新的内容，因为指针现在指向了一个完全不同的节点。节点`B`并没有被编辑、更新或者修改，取而代之的是，我们创建了一个新的DAG，其中指针指向了CID`E`，它是由节点`A`和一个新的节点`D`组合而成的。

```
   +-----------+
   |  Pointer  | --------------+
   +-----------+               |
                               ↓
      +-----+               +-----+
   +--|  C  |-+         +-- |  E  | --+
   |  +-----+ |         |   +-----+   |
   |          |         |             |
+-----+    +-----+     +-----+    +-----+
|  A  |    |  B  |     |  A  |    |  D  |
+-----+    +-----+     +-----+    +-----+
"hello"    "world"     "hello"    "IPFS!"
```

再次强调，节点`B`没有改变。它总是指向相同的内容，即`world`。节点`A`也出现在新的DAG中，这并不一定意味着我们把包含`hello`字符串的内存/缓冲区拷贝到了新消息中，这会意味着位置寻址的范式，即关注其位置而非其内容。在内容寻址的系统中，任何时候人们写入一个内容为`"hello"`的块，它的CID都总是`A`，无论我们是从之前位置拷贝过来的字符串还是重新创建的字符串。

## 网站说明

这里我们有一个网站，显示了两个标题分别为`header_1`和`header_2`。标题的内容分别由变量`string_1`和`string_2`提供。

```html
<body>
  <h1 id="header_1"></h1>
  <h1 id="header_2"></h1>
</body>
<script>
  let string_1 = 'hello'
  let string_2 = 'world'
  document.getElementById('header_1').textContent = string_1
  document.getElementById('header_2').textContent = string_2
</script>
```

该网站的CID为`QmWLdyFMUugMtKZs1xeJCSUKerWd9M627gxjAtp6TLrAgP`。可以打开[`example.com/QmWLdyFMUugMtKZs1xeJCSUKerWd9M627gxjAtp6TLrAgP`](https://gateway.pinata.cloud/ipfs/QmWLdyFMUugMtKZs1xeJCSUKerWd9M627gxjAtp6TLrAgP)来访问该网站。如果我们把`string_2`修改为`IPFS`，此时网站的CID就会修改为`Qme1A6ofTweQ1JSfLLdkoehHhpbAAk4Z2hWjyNC7YJF9m5`，这是我们就要打开[`example.com/Qme1A6ofTweQ1JSfLLdkoehHhpbAAk4Z2hWjyNC7YJF9m5`](https://gateway.pinata.cloud/ipfs/Qme1A6ofTweQ1JSfLLdkoehHhpbAAk4Z2hWjyNC7YJF9m5)来访问它了。

让用户通过CID来访问网站将会很麻烦，因为每次变量更新的时候，CID都会改变。因此我们可以使用一个指针来维护拥有最新更新页面的CID。这样用户可以访问`example.com`，然后总能被指向最新的内容。这个指针是可变的，它可以被更新来反映下游的更新。

```
+--------+      +---------+      +----------+
|  User  | ---> | Pointer | ---> | QmWLd... |
+--------+      +---------+      +----------+
```

在这个网站的示例中，我们修改变量的时候，网页的CID就会不同。其指针需要被更新以重定向用不到最新的网页。重要的一点是原有的CID依然存在，没有任何内容会被覆盖重写。原始的CID`QmWLdyFMUugMtKZs1xeJCSUKerWd9M627gxjAtp6TLrAgP`总会指向具有`hello`和`world`标题的页面。我们只是构建了一个新的[DAG](/concepts/merkle-dag)。

```
+--------+      +---------+      +----------+
|  User  | ---> | Pointer |      | QmWLd... |
+--------+      +---------+      +----------+
                     |
                     |           +----------+
                     + --------> | Qme1A... |
                                 +----------+
```

这个过程就是[星际名字服务(IPNS)](/concepts/ipns)本质上在做的事情！CID很难处理，也很难记住，因此IPNS让用户免于直接处理CID。更重要的是，CID会随着内容而发生改变，但是入站引用的URL/指针可以保持不变，而出站引用会发生变化！

```
+--------+      +--------------+      +-------------------------------------------------------------+
|  User  | ---> | docs.ipfs.io | ---> | bafybeigsddxhokzs3swgx6mss5i3gm6jqzv5b45e2xybqg7dr3jmsykrku |
+--------+      +--------------+      +-------------------------------------------------------------+
```
