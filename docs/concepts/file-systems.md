---
title: 文件系统
legacyUrl: https://docs.ipfs.io/guides/concepts/mfs/
description: Learn about file systems in IPFS and why working with files in IPFS can be a little different than you may be used to.
---

# 文件系统与IPFS

在IPFS中操作文件与通常的方式会有所不同，主要基于以下几点原因：

- 内容寻址意味着当文件被修改时，这些文件的内容标识符（CID）也发生了变化。
- 文件可能因为太多而无法放入单一的块中，此时IPFS将数据切分为多个块，并通过元数据将其链接在一起。

可变文件系统 Mutable File System (MFS)和 Unix文件系统 Unix File System (UnixFS)可以帮助你了解这些对文件的新处理方式。

::: tip
如果对MFS和UnixFS是如何参与到IPFS的文件处理中感兴趣，可以查看这个IPFS Camp 2019的视频！[Core Course: How IPFS Deals With Files](https://www.youtube.com/watch?v=Z5zNPwMDYGg)
:::

## 可变文件系统 (MFS)

因为IPFS中的文件是内容寻址，且不可变的，要编辑它们会很复杂。可变文件系统(MFS)是IPFS自带的工具，让你可以像处理常规的基于名称的文件系统一样来处理其文件，你可以添加、删除、移动和编辑MFS文件，MFS会帮你处理好包括更新链接及hash的工作。

### 使用文件API

MFS可以通过IPFS CLI和API的文件命令来访问。文件API的命令格式为`ipfs.files.someMethod()`。其方法可以用于：

- [创建目录](#创建目录)
- [检查目录状态](#检查目录状态)
- [添加文件到MFS](#添加文件到MFS)
- [查看目录内容](#查看目录内容)
- [复制文件或目录](#复制文件或目录)
- [移动文件或目录](#移动文件或目录)
- [读取文件内容](#读取文件内容)
- [删除文件或目录](#删除文件或目录)

::: callout
更喜欢上手操作来学习的话，可以在ProtoSchool的[Mutable File System](https://proto.school/mutable-file-system)教程中探索这些MFS方法，直接在你的浏览器中完成这些代码挑战。
:::

#### 创建目录

MFS方法`ipfs.files.mkdir`在指定路径下创建了一个新目录。例如要在根目录(`/`)下添加目录`example`，运行：

```
await ipfs.files.mkdir('/example')
```

如果要在当前不存在的目录下创建一个新子目录，需要明确设置`parents`值为`true`，如下：

```
await ipfs.files.mkdir('/my/directory/example', { parents: true })
```

#### 检查目录状态

`ipfs.files.stat`方法让你能够检查IPFS节点的文件或目录状态。要检查根目录下名为`example`的目录状态，可以这样调用方法：

```
await ipfs.files.stat('/example')
```

该方法返回一个对象，其中包含`cid`, `size`, `cumulativeSize`, `type`, `blocks`, `withLocality`, `local`, 以及`sizeLocal`。

```
// {
//   hash: CID('QmXmJBmnYqXVuicUfn9uDCC8kxCEEzQpsAbeq1iJvLAmVs'),
//   size: 60,
//   cumulativeSize: 118,
//   blocks: 1,
//   type: 'directory'
// }
```

如果你在目录中添加、移动、复制或删除了一个文件，该目录的hash会伴随每个文件的修改而发生改变。

#### 添加文件到MFS

要添加一个文件到MFS中，使用`ipfs.files.write`方法来执行命令：

```
await ipfs.files.write(path, content, [options])
```

::: tip

该方法可以接受多种格式的`content`，通过指定{`create: true`}选项，可以在指定的`path`中创建全新的文件。

:::

要将`examplePic`文件对象添加到根路径下，可以运行：

```
await ipfs.files.write('/example.jpg', examplePic, { create: true })
```

该方法没有返回值。

#### 查看目录内容

要验证`ipfs.files.write`方法是否如预期方式工作，可以使用`ipfs.files.ls`方法来检查MFS中的目录：

```bash
await ipfs.files.ls([path], [options])
```

该方法默认会列出(`/`)目录的内容，也可以通过`path`指定要检查的目录：

```bash
await ipfs.files.ls('/example')
```

该方法为每个文件/目录生成了一个对象数组，包含了如`name`, `type`, `size`, `cid`, `mode`, 和`mtime`等属性。如果要检查`/example`目录的内容，可以运行：

```
await ipfs.files.ls('/example')
```

#### 复制文件或目录

在可变文件系统中，和传统文件系统一样，可以复制一个文件或目录到新的位置，同时源文件保持不变。

可以使用该方法来操作：

```
await ipfs.files.cp(...from, to, [options])
```

::: tip

该方法提供了两种方式来传入`from`信息：

- 一个你的节点中已有文件/目录的MFS路径(如：`/my-example-dir/my-example-file.txt`)
- 一个由你或者其他节点托管的文件/目录的IPFS路径(如：`/ipfs/QmWc7U4qGeRAEgtsyVyeW2CRVbkHW31nb24jFyks7eA2mF`)

:::

`to`表示的是MFS中的目标路径，还支持{`create: true` }选项，可以用于父目录不存在时自动创建。

可以使用该方法来执行不同操作，包括：

```
// 复制单一文件到目录中
await ipfs.files.cp('/example-file.txt', '/destination-directory')
await ipfs.files.cp('/ipfs/QmWGeRAEgtsHW3ec7U4qW2CyVy7eA2mFRVbk1nb24jFyks', '/destination-directory')

// 复制多个文件到目录中
await ipfs.files.cp('/example-file-1.txt', '/example-file-2.txt', '/destination-directory')
await ipfs.files.cp('/ipfs/QmWGeRAEgtsHW3ec7U4qW2CyVy7eA2mFRVbk1nb24jFyks',
 '/ipfs/QmWGeRAEgtsHW3jk7U4qW2CyVy7eA2mFRVbk1nb24jFyre', '/destination-directory')

// 复制一个目录到目录中
await ipfs.files.cp('/source-directory', '/destination-directory')
await ipfs.files.cp('/ipfs/QmWGeRAEgtsHW3ec7U4qW2CyVy7eA2mFRVbk1nb24jFyks', '/destination-directory')
```

#### 移动文件或目录

MFS允许使用`ipfs.files.mv`方法在目录间移动文件，类似于：

```
await ipfs.files.mv(from, to, [options])
```

`from`是要移动的源路径（多个源路径），`to`则是目标路径。

可以使用该方法来执行不同操作，包括：

```
// 将单个文件移到目录中
await ipfs.files.mv('/example-file.txt', '/destination-directory')

// 将目录移到另一个目录中
await ipfs.files.mv('/source-directory', '/destination-directory')

// 用源文件覆盖目标文件的内容
await ipfs.files.mv('/source-file.txt', '/destination-file.txt')

// 将多个文件移到目录中
await ipfs.files.mv('/example-file-1.txt', '/example-file-2.txt', '/example-file-3.txt', '/destination-directory')
```

#### 读取文件内容

`ipfs.files.read`方法允许读取或显示文件的内容到一个buffer中。该方法格式如下：

```
ipfs.files.read(path, [options])
```

`path`表示需要读取的文件路径，它必须指向一个文件，而非一个目录。

#### 删除文件或目录

MFS允许通过以下方法删除文件或目录：

```
await ipfs.files.rm(...paths, [options])
```

`paths`是一个或多个待删除的路径。

默认情况下，如果要删除还有内容的目录，该请求会失败。为了删除一个目录及其中包含的所有内容，需要使用`recursive: true`选项。

```
// 删除单个文件
await ipfs.files.rm('/my/beautiful/file.txt')

// 删除多个文件
await ipfs.files.rm('/my/beautiful/file.txt', '/my/other/file.txt')

// 删除一个目录及其中的内容
await ipfs.files.rm('/my/beautiful/directory', { recursive: true })

// 仅在目录为空时删除目录
await ipfs.files.rm('/my/beautiful/directory')
```

## Unix文件系统(UnixFS)

当你将一个文件添加到IPFS中时，它可能因为过大无法放到单一块中，此时需要元数据来把这些块链接起来。UnixFS是一个基于[protocol-buffers](https://developers.google.com/protocol-buffers/)的格式，用于在IPFS中描述文件、目录和符号链接。该数据格式可以用于表示IPFS中的文件和它的所有链接及元数据。UnixFS创建链接对象的块（或者块树）。

UnixFS当前有[Javascript](https://github.com/ipfs/js-ipfs-unixfs)和[Go](https://github.com/ipfs/go-ipfs/tree/b3faaad1310bcc32dc3dd24e1919e9edf51edba8/unixfs)的实现。这些实现都具有运行不同功能的模块：

- **数据格式**: 管理UnixFS对象到protocol buffer的序列化/反序列化

- **导入器**: 为文件和目录构建DAG

- **导出器**: 导出DAG

### 数据格式

在UnixFS-v1中数据格式会表示为此protobuf：

```
message Data {
    enum DataType {
        Raw = 0;
        Directory = 1;
        File = 2;
        Metadata = 3;
        Symlink = 4;
        HAMTShard = 5;
    }

    required DataType Type = 1;
    optional bytes Data = 2;
    optional uint64 filesize = 3;
    repeated uint64 blocksizes = 4;
    optional uint64 hashType = 5;
    optional uint64 fanout = 6;
    optional uint32 mode = 7;
    optional UnixTime mtime = 8;
}

message Metadata {
    optional string MimeType = 1;
}

message UnixTime {
    required int64 Seconds = 1;
    optional fixed32 FractionalNanoseconds = 2;
}
```

`Data`对象用于UnixFS的所有非叶子节点：

- 对于有多个块组成的文件，`Type`字段设为`File`，`filesize`字段设为文件的总字节数，且`blocksizes`会包含一个列表，其中为每个子节点的filesize。

- 对于单个块组成的文件，`Type`字段设为`File`，`filesize`设为文件的总字节数，且文件数据存储于`Data`字段中。

UnixFS支持两个可选的元数据格式字段：

- `mode` - 用于以[数字表示法](https://en.wikipedia.org/wiki/File_system_permissions#Numeric_notation)保留文件的权限信息。如果未指定，该字段的默认值对目录/HAMT分片为`0755`，对所有其他适用类型为`0644`。

- `mtime` - 是一个有两个元素的结构(`Seconds`, `FractionalNanoseconds`)，表示相对于Unix纪元`1970-01-01T00:00:00Z`的修改时间，以秒为单位。

### 导入器

将文件导入到UnixFS中分为两个过程：一个是分块，另一个是布局。可以使用IPFS [DAG builder](https://dag.ipfs.io)来测试这些特性。

#### 分块

当一个对象添加到IPFS中的时候，它被分成更小的部分，每个块都进行了hash运算，并为之创建了一个CID。这个DAG构建过程有两个主要的参数：叶子格式和分块策略。

叶子格式有两个格式选项：UnixFS叶和原始叶：

- UnixFS叶子格式在新增的对象上添加了一个数据包装器，从而生成了有额外数据大小的UnixFS叶子。该包装器用于判断新增的对象是文件还是目录，该格式是CIDv0的默认格式。

- 原始叶子格式指IPFS中，对文件进行分片后输出的各个节点都是其原始数据，其CID也是基于原始数据编码的。该配置主要是为了用于向后兼容来使用UnixFS数据对象的格式。这是通过`ipfs add --cid-version 1`命令生成的CIDv1的默认格式，很快也会称为全局默认格式。

分块策略用于确定在分块过程中可用的大小选项。该策略当前有两个不同的选项：'固定大小' 和 'rabin'。

- 固定大小会将输入的数据按指定大小分片。这个值可以是512 bytes, 1024 bytes等等；字节大小越小，去除重复数据的效果越好。

- Rabin分块会使用Rabin指纹来确定块之间的边界，从而完成输入数据的分块。Rabin同时还减少了输入数据分块节点的数量。

#### 布局

布局定义了从输入文件的块所构建出来的树的形状。

当前对布局有两个选项：平衡和trickle。另外，还要指定`max-width`最大宽度值，其默认宽度为174。

平衡布局创建了一个宽度为`max-width`的平衡树。该树的构建方式是从块数据流中获取`max-width`数量的块，然后创建一个链接到所有这些块的UnixFS文件节点；重复这个过程，直到已经创建了`max-width`数量的UnixFS文件节点，然后又递归式的再创建一个UnixFS节点，以链接这些`max-width`数量的UnixFS文件节点。最后结果树的根节点会作为这个新导入文件的句柄返回。

如果只有一个块，不会再创建中间的UnixFS文件节点，这个单块会作为文件的句柄直接返回。

### 导出器

为了从UnixFS图中导出和读取文件数据，可以执行有序遍历，从每个叶子节点中依次取出其包含的数据。

## 更多资源

可以在此找到更多的资源以熟悉这些文件系统：

- [Protoschool MFS tutorial](https://proto.school/mutable-file-system)
- [Understanding how the InterPlanetary File System deals with Files](https://github.com/ipfs/camp/tree/master/CORE_AND_ELECTIVE_COURSES/CORE_COURSE_A), from IPFS Camp 2019
- [Jeromy Coffee Talks - Files API](https://www.youtube.com/watch?v=FX_AXNDsZ9k)
- [UnixFS Specification](https://github.com/ipfs/specs/blob/master/UNIXFS.md)
