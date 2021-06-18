---
title: 配置节点
description: IPFS nodes can be customzied using the configuration file. The default values should be fine for most use-cases. However, you may want to make some changes if you are running a specialized IPFS node, or simply want to tweak things to your liking.
---

# 配置节点

IPFS配置文件是json格式，默认位于`~/.ipfs/config`。与实现相关的特定信息可以在[go-ipfs](https://github.com/ipfs/go-ipfs/blob/master/docs/config.md)和[js-ipfs](https://github.com/ipfs/js-ipfs/blob/master/docs/CONFIG.md)仓库中找到。它会在节点实例化时被读取加载一次，包括在离线命令调用或作为守护进程启动的时候。在正在运行的守护进程中执行命令，不会读取配置文件。

## 配置选项profile

配置选项允许你快速调整配置。可以在`ipfs init`中指定 `--profile`标识或者在`ipfs config profile apply`命令中应用配置选项。当应用一个配置选项时，会在`$IPFS_PATH`路径下创建一个当前配置的备份。

可用的配置选项参见如下，也可以通过`ipfs config profile --help`查看对应信息：

- `server`

  禁用本地节点发现，当IPFS所在的主机拥有公网IPv4地址时，建议采用此配置。

- `randomports`

  使用随机的swarm端口号。

- `default-datastore`

  配置节点为使用默认数据存储（flatfs）。

  关于此数据存储的信息，阅读"flatfs"配置项的说明。

  该配置项只能在第一次初始化节点时使用。

- `local-discovery`

  将受server配置选项影响的字段设为默认值，以启用本地网络发现功能。

- `test`

  减少IPFS守护进程的外部干扰，在测试环境中使用守护进程时有用。

- `default-networking`

  恢复默认的网络设置。
  反转test的配置。

- `flatfs`

  配置节点以采用flatfs数据存储。

  这是最久经考验和最可靠的数据存储，但它相对badger数据存储明显要慢。应该在以下情形下使用此数据存储：

  - 需要一个极其简单又极其可靠的数据存储，并且信任你的文件系统。此数据存储将每个块数据作为独立文件存储在底层文件系统中，因此除非底层文件系统存在问题，否则不太可能丢失数据。
  - 需要在一个较小的(<= 10GiB)数据存储中运行垃圾回收。默认的数据存储，badger，在垃圾回收时可能会产生若干G的数据。
  - 需要关注内存使用状况时。在默认的配置中，badger可能会使用高达数G的内存。

  该配置项只能在第一次初始化节点时使用。

- `badgerds`

  配置节点以采用badger数据存储。

  这是最快的数据存储。如果特别关注性能，尤其是添加许多GB的文件数据时的性能，此数据存储非常有用。然而：

  - 当你的数据存储少于一定的GB值时，该数据存储可能无法回收存储空间。如果你使用'--enable-gc'参数（从而启用块级别的垃圾回收）来运行IPFS，计划在IPFS节点中存储非常少的数据，并且磁盘占用率优先于性能考虑时，应当考虑使用flatfs。
  - 此数据存储会占用最多若干GB的内存。

  该配置项只能在第一次初始化节点时使用。

- `lowpower`

  减少守护进程的系统开销。这可能会影响到节点的功能 - 如内容发现和数据获取的性能可能会降低。

## 取值类型Types

配置文件使用标准JSON类型（如`null`, `string`, `number`等），以及一些新的自定义类型数据，如下描述：

### `flag`

Flags允许启动或者禁用特性。与普通的布尔值不同，此类型可以为`null` (或者省略值)，表示使用默认值。这使得go-ipfs后续可以更容易的修改默认值，除非用户 _明确的_ 将flag设为`true`(启用)或者`false`(禁用)。Flags取三种可能值：

- `null` 或缺失 (使用默认值)
- `true` (启用)
- `false` (禁用)

### `priority`

Priorities可以指定特性/协议的优先级，也可以禁用特性/协议。Priorities可以取以下值：

- `null`/缺失 (使用默认的优先级，同flags)
- `false` (禁用)
- `1 - 2^63` (优先级取值，数值越低优先级越高)

### `strings`

Strings是一个特殊类型，用于更方便的指定单个字符串、字符串数组或者空值：

- `null`
- `"a single string"`
- `["an", "array", "of", "strings"]`

### `duration`

Duration用于描述时间长度，使用Go的时间长度格式。(如 `"1d2h4m40.01s"`)。

## `Addresses`

包含了本节点使用的各种监听地址信息。

### `Addresses.API`

多重地址或者多重地址数组，用以描述本地HTTP API服务的地址。

支持的传输协议：

- tcp/ip{4,6} - `/ipN/.../tcp/...`
- unix - `/unix/path/to/socket`

默认值：`/ip4/127.0.0.1/tcp/5001`

值类型：`strings` (多重地址)

### `Addresses.Gateway`

多重地址或者多重地址数组，用以描述本地网关服务的地址。

支持的传输协议：

- tcp/ip{4,6} - `/ipN/.../tcp/...`
- unix - `/unix/path/to/socket`

默认值：`/ip4/127.0.0.1/tcp/8080`

值类型：`strings` (多重地址)

### `Addresses.Swarm`

多重地址数组，用以描述监听p2p swarm连接的地址。

支持的传输协议：

- tcp/ip{4,6} - `/ipN/.../tcp/...`
- websocket - `/ipN/.../tcp/.../ws`
- quic - `/ipN/.../udp/.../quic`

默认值：

```json
[
  "/ip4/0.0.0.0/tcp/4001",
  "/ip6/::/tcp/4001",
  "/ip4/0.0.0.0/udp/4001/quic",
  "/ip6/::/udp/4001/quic"
]
```

值类型：`array[string]` (多重地址)

### `Addresses.Announce`

非空时，指定一个广播到网络中的swarm地址数组。
取空值时，守护进程会自行推定其swarm地址数组并广播。

默认值：`[]`

值类型：`array[string]` (多重地址)

### `Addresses.NoAnnounce`

指定不要广播到网络中的swarm地址数组。

默认值：`[]`

值类型：`array[string]` (多重地址)

## `API`

包含API网关会使用的信息。

### `API.HTTPHeaders`

API HTTP服务器响应中会包含的HTTP头字段映射表。

示例：

```json
{
  "Foo": ["bar"]
}
```

默认值：`null`

值类型：`object[string -> array[string]]` (头字段名称 -> 头字段取值数组)

## `AutoNAT`

包含AutoNAT服务的配置选项。AutoNAT为网络中的其他节点提供服务，帮助它们确定自己是否可以被互联网中其他节点公开访问到。

### `AutoNAT.ServiceMode`

（默认）未设置时，AutoNAT值为 _enabled_。否则该字段可以取以下值：

- "enabled" - 启用服务（除非节点检测到自己无法被公网直接访问）。
- "disabled" - 禁用服务。

未来可能会扩展更多的模式。

值类型：`string` (`"enabled"`或`"disabled"`二者取一)

### `AutoNAT.Throttle`

该选项用于设置AutoNAT服务的限制行为。
默认go-ipfs会限制给其他节点进行NAT检测的速率为：每分钟最多检测30次，其中每个节点最多检测3次。

### `AutoNAT.Throttle.GlobalLimit`

配置每个`AutoNAT.Throttle.Interval`周期内所提供的AutoNAT服务请求数。

默认值：30

值类型：`integer` (非负值，`0`表示不限制)

### `AutoNAT.Throttle.PeerLimit`

配置每个`AutoNAT.Throttle.Interval`周期内为每个节点提供的AutoNAT服务请求数。

默认值：3

值类型：`integer` (非负值，`0`表示不限制)

### `AutoNAT.Throttle.Interval`

配置上面AutoNAT限定所对应的一个周期的时长。

默认值：1 Minute

值类型：`duration` (取`0`/未设定时，使用默认值)

## `Bootstrap`

Bootstrap是一组信任节点的多重地址列表，用于初始化时更快的连接到网络中。

默认值：ipfs.io提供的bootstrap节点

值类型：`array[string]` (多重地址)

## `Datastore`

包含与磁盘存储系统构建和操作相关的信息。

### `Datastore.StorageMax`

IPFS仓库数据存储的软上限值。与`StorageGCWatermark`选项一起可以用于计算是否需要触发垃圾回收机制的运行(仅在指定`--enable-gc`标识的时候)。

默认值：`"10GB"`

值类型：`string` (size)

### `Datastore.StorageGCWatermark`

`StorageMax`的百分比值，用于指定当守护进程启用了垃圾回收时（当前该选项默认为禁用），何时会自动触发垃圾回收。

默认值：`90`

值类型：`integer` (0-100%)

### `Datastore.GCPeriod`

一个时间长度值，指定垃圾回收的运行频度。仅在垃圾回收启用时生效。

默认值：`1h`

值类型：`duration` (空值表示使用默认值)

### `Datastore.HashOnRead`

布尔值。设为true时，从磁盘读取块数据时会计算其hash并校验。这会提升CPU占用。

默认值：`false`

值类型：`bool`

### `Datastore.BloomFilterSize`

指定块存储的[布隆过滤器（bloom filter）](https://en.wikipedia.org/wiki/Bloom_filter)的字节数大小。取值为0表示关闭该特性。

这个网站已经生成了一些有用的各种布隆过滤器取值所对应的图表数据：<https://hur.st/bloomfilter/?n=1e6&p=0.01&m=&k=7>。可以使用它来找到一个首选最优值，其中`m`对应于`BloomFilterSize`的比特位长。记住配置文件中要把比特位单位的`m`取值转换为字节单位的`BloomFilterSize`大小。例如，对于1,000,000个块数据，希望低于1%的假正例概率，最终会得到过滤器的大小为9592955 bits，因此`BloomFilterSize`应该取值为1199120 bytes。这里使用的是[7 hash functions](https://github.com/ipfs/go-ipfs-blockstore/blob/547442836ade055cc114b562a3cc193d4e57c884/caching.go#L22)，因此公式中常量`k`取值为7。

默认值：`0` (disabled)

值类型：`integer` (非负值，bytes)

### `Datastore.Spec`

Spec定义了ipfs数据存储的结构。这是一个可组合的结构，其中每个数据存储由一个json对象来表示。数据存储中还可以封装其他的数据存储，从而提供额外的功能（如指标、日志或缓存）。

这个可以手动修改，然而，如果修改导致了任何磁盘存储结构的变化，需要运行[ipfs-ds-convert
tool](https://github.com/ipfs/ipfs-ds-convert)工具来将数据迁移到新的结构中。

默认值：

```
{
  "mounts": [
	{
	  "child": {
		"path": "blocks",
		"shardFunc": "/repo/flatfs/shard/v1/next-to-last/2",
		"sync": true,
		"type": "flatfs"
	  },
	  "mountpoint": "/blocks",
	  "prefix": "flatfs.datastore",
	  "type": "measure"
	},
	{
	  "child": {
		"compression": "none",
		"path": "datastore",
		"type": "levelds"
	  },
	  "mountpoint": "/",
	  "prefix": "leveldb.datastore",
	  "type": "measure"
	}
  ],
  "type": "mount"
}
```

值类型：`object`

## `Discovery`

包含了配置ipfs节点发现机制所对应的选项。

### `Discovery.MDNS`

多播DNS节点发现的选项。

#### `Discovery.MDNS.Enabled`

布尔值，指定是否启用mdns。

默认值：`true`

值类型：`bool`

#### `Discovery.MDNS.Interval`

mdns发现的检测间隔值，单位为秒。

默认值：`5`

值类型：`integer` (整数秒值, 0表示默认值)

## `Gateway`

HTTP网关的选项。

### `Gateway.NoFetch`

设为true时，网关仅提供本地仓库已有的文件，不再从网络中去获取文件。

默认值：`false`

值类型：`bool`

### `Gateway.NoDNSLink`

布尔值，配置DNSLink是否去查询HTTP头中的`Host`字段对应的值。
A boolean to configure whether DNSLink lookup for value in `Host` HTTP header
should be performed. If DNSLink is present, content path stored in the DNS TXT
record becomes the `/` and respective payload is returned to the client.

默认值：`false`

值类型：`bool`

### `Gateway.HTTPHeaders`

网关响应中包含的头信息。

默认值：

```json
{
  "Access-Control-Allow-Headers": ["X-Requested-With"],
  "Access-Control-Allow-Methods": ["GET"],
  "Access-Control-Allow-Origin": ["*"]
}
```

值类型：`object[string -> array[string]]`

### `Gateway.RootRedirect`

将`/`请求重定向到此url。

默认值：`""`

值类型：`string` (url)

### `Gateway.Writable`

布尔值，配置网关是否可写。

默认值：`false`

值类型：`bool`

### `Gateway.PathPrefixes`

Array of acceptable url paths that a client can specify in X-Ipfs-Path-Prefix
header.

The X-Ipfs-Path-Prefix header is used to specify a base path to prepend to links
in directory listings and for trailing-slash redirects. It is intended to be set
by a frontend http proxy like nginx.

Example: We mount `blog.ipfs.io` (a dnslink page) at `ipfs.io/blog`.

**.ipfs/config**

```json
"Gateway": {
  "PathPrefixes": ["/blog"],
}
```

**nginx_ipfs.conf**

```nginx
location /blog/ {
  rewrite "^/blog(/.*)$" $1 break;
  proxy_set_header Host blog.ipfs.io;
  proxy_set_header X-Ipfs-Gateway-Prefix /blog;
  proxy_pass http://127.0.0.1:8080;
}
```

默认值：`[]`

值类型：`array[string]`

### `Gateway.PublicGateways`

`PublicGateways`是一个字典，定义了针对特定的主机名，网关所对应的行为。

主机名支持一个或多个通配符。

示例：

- `*.example.com`可以匹配`http://foo.example.com/ipfs/*`或者`http://{cid}.ipfs.bar.example.com/*`。
- `foo-*.example.com`可以匹配`http://foo-bar.example.com/ipfs/*`或者`http://{cid}.ipfs.foo-xyz.example.com/*`。

#### `Gateway.PublicGateways: Paths`

一组可以在主机名上公开的路径。

示例：

```json
{
  "Gateway": {
    "PublicGateways": {
      "example.com": {
        "Paths": ["/ipfs", "/ipns"]
      }
    }
  }
}
```

以上启用了`http://example.com/ipfs/*`和`http://example.com/ipns/*`，但没有启用`http://example.com/api/*`

默认值：`[]`

值类型：`array[string]`

#### `Gateway.PublicGateways: UseSubdomains`

布尔值，配置主机名所对应的网关是否对不同的内容数据提供[源隔离](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)。

- `true` - 在`http://*.{hostname}/`上启用[子域名网关](#https://docs.ipfs.io/how-to/address-ipfs-on-web/#subdomain-gateway)

  - **白名单需求：** 确保设置了相应的`Paths`值。
    例如，需要指定`Paths: ["/ipfs", "/ipns"]`，以使得`http://{cid}.ipfs.{hostname}`和`http://{foo}.ipns.{hostname}`路径有效：
    ```json 
    "Gateway": { 
      "PublicGateways": { 
        "dweb.link": { 
          "UseSubdomains": true, 
          "Paths": ["/ipfs", "/ipns"], 
        } 
      } 
    } 
    ```
  - **向后兼容：** 对内容路径如`http://{hostname}/ipfs/{cid}`的请求，会重定向到`http://{cid}.ipfs.{hostname}`
  - **API：** 如果`Paths`白名单中包含`/api`，`http://{hostname}/api/{cmd}`会重定向到`http://api.{hostname}/api/{cmd}`

- `false` - 在`http://{hostname}/*`上启用[路径网关](https://docs.ipfs.io/how-to/address-ipfs-on-web/#path-gateway)
  - 示例：
  ```json
  "Gateway": {
    "PublicGateways": {
      "ipfs.io": {
        "UseSubdomains": false,
        "Paths": ["/ipfs", "/ipns", "/api"], 
      } 
    } 
  } 
  ```
  <!-- **(not implemented yet)** due to the lack of Origin isolation, cookies and storage on `Paths` will be disabled by [Clear-Site-Data](https://github.com/ipfs/in-web-browsers/issues/157) header -->

默认值：`false`

值类型：`bool`

#### `Gateway.PublicGateways: NoDNSLink`

布尔值，配置该主机名下HTTP的`Host`头的DNSLink是否解析。覆盖全局配置，如果定义了`Paths`，则优先使用`Paths`。

默认值：`false` (默认对每个主机名启用DNSLink查询)

值类型：`bool`

#### `Gateway.PublicGateways`隐式默认值

`localhost`主机名和回环IP的入口都有默认配置。
如果为这些主机名提供了额外的配置，则会和隐含的默认值合并：

```json
{
  "Gateway": {
    "PublicGateways": {
      "localhost": {
        "Paths": ["/ipfs", "/ipns"],
        "UseSubdomains": true
      }
    }
  }
}
```

也可以将默认设置设为`null`以移除。如要禁用`localhost`的子域名网关，并使它与`127.0.0.1`表现一致：

```shell
$ ipfs config --json Gateway.PublicGateways '{"localhost": null }'
```

### `Gateway` recipes

以下是最常见的公共网关配置。

- `http://{cid}.ipfs.dweb.link`上的公共[子域名网关](https://docs.ipfs.io/how-to/address-ipfs-on-web/#subdomain-gateway) (每个根内容有自己的源)

  ```shell
  $ ipfs config --json Gateway.PublicGateways '{
      "dweb.link": {
        "UseSubdomains": true,
        "Paths": ["/ipfs", "/ipns"]
      }
    }'
  ```

  **向后兼容：** 该特性启用了内容路径到子域名的自动重定向：
  `http://dweb.link/ipfs/{cid}` → `http://{cid}.ipfs.dweb.link`
  **X-Forwarded-Proto:** 如果你是在提供TLS的反向代理后面运行的go-ipfs，需要配置添加`X-Forwarded-Proto: https` HTTP头，以确保用户会被重定向到`https://`而不是`http://`上。还要确保DNSLink名称是内联的（inlined）以适配单DNS标签，这样才能与通配符TLS证书正常工作([详情](https://github.com/ipfs/in-web-browsers/issues/169))。NGINX对应的配置是：`proxy_set_header X-Forwarded-Proto "https";`.:
  `http://dweb.link/ipfs/{cid}` → `https://{cid}.ipfs.dweb.link`
  `http://dweb.link/ipns/your-dnslink.site.example.com` → `https://your--dnslink-site-example-com.ipfs.dweb.link`
  **X-Forwarded-Host:** 如果想覆盖子域名网关的主机名，也支持通过`X-Forwarded-Host: example.com`来实现：
  `http://dweb.link/ipfs/{cid}` → `http://{cid}.ipfs.example.com`

- `http://ipfs.io/ipfs/{cid}` 上的公共[路径网关](https://docs.ipfs.io/how-to/address-ipfs-on-web/#path-gateway) (没有源隔离)

  ```shell
  $ ipfs config --json Gateway.PublicGateways '{
      "ipfs.io": {
        "UseSubdomains": false,
        "Paths": ["/ipfs", "/ipns", "/api"]
      }
    }'
  ```

- 公共[DNSLink](https://dnslink.io/)网关，会解析在`Host`头中传入的每个主机名。

  ```shell
  $ ipfs config --json Gateway.NoDNSLink true
  ```

  - 默认为`NoDNSLink: false` (it works out of the box unless set to `true` manually)

- Hardened, site-specific [DNSLink gateway](https://docs.ipfs.io/how-to/address-ipfs-on-web/#dnslink-gateway).
  禁用获取远程数据 (`NoFetch: true`)
  解析未知主机名的DNSLink (`NoDNSLink: true`).
  然后，只对特定的主机名启用DNSLink网关(for which data is already present on the node)，不对外暴露任何基于内容寻址的路径：
  "NoFetch": true,
  "NoDNSLink": true,
  ```shell
  $ ipfs config --json Gateway.NoFetch true
  $ ipfs config --json Gateway.NoDNSLink true
  $ ipfs config --json Gateway.PublicGateways '{
      "en.wikipedia-on-ipfs.org": {
        "NoDNSLink": false,
        "Paths": []
      }
    }'
  ```

## `Identity`

### `Identity.PeerID`

当前配置节点的唯一PKI标识标签。它在初始化时设置，后续也不会再被访问。这里仅仅是为了方便使用。IPFS在运行时会基于它的密钥对来生成节点ID。

值类型：`string` (peer ID)

### `Identity.PrivKey`

base64编码的protobuf内容，包含了节点的私钥。

值类型：`string` (base64 encoded)

## `Ipns`

### `Ipns.RepublishPeriod`

时间长度值，指定重新发布ipns记录的时间频度，以确保它在网络中持续有效。

默认值：4 hours.

值类型：`interval` 或者空值，表示使用默认值。

### `Ipns.RecordLifetime`

时间长度值，指定ipns记录的有效时长。

默认值：24 hours.

值类型：`interval` 或者空值，表示使用默认值。

### `Ipns.ResolveCacheSize`

The number of entries to store in an LRU cache of resolved ipns entries. Entries
will be kept cached until their lifetime is expired.

默认值：`128`

值类型：`integer` (non-negative, 0 means the default)

## `Mounts`

FUSE挂载点的配置选项。

### `Mounts.IPFS`

`/ipfs/`的挂载点。

默认值：`/ipfs`

值类型：`string` (filesystem path)

### `Mounts.IPNS`

`/ipns/`的挂载点

默认值：`/ipns`

Type: `string` (filesystem path)

### `Mounts.FuseAllowOther`

Sets the FUSE allow other option on the mountpoint.

## `Pinning`

Pinning可以配置固定内容相关的选项。
(如保持内容长期有效，而不是临时缓存)。

### `Pinning.RemoteServices`

`RemoteServices` 提供了一组名称和对应的远程固定服务及其配置的映射表。

远程固定服务是一个远程服务，它公开了API，用于管理该服务所关注的长期数据存储。

公开的API需要符合在[pinning-services-api-spec](https://ipfs.github.io/pinning-services-api-spec/) 定义的规范。


#### `Pinning.RemoteServices: API`

包含与使用该远程固定服务相关的信息

示例：

```json
{
  "Pinning": {
    "RemoteServices": {
      "myPinningService": {
        "API": {
          "Endpoint": "https://pinningservice.tld:1234/my/api/path",
          "Key": "someOpaqueKey"
        }
      }
    }
  }
}
```

##### `Pinning.RemoteServices: API.Endpoint`

HTTP(S)入口，用于方案固定服务

示例："https://pinningservice.tld:1234/my/api/path"

值类型：`string`

##### `Pinning.RemoteServices: API.Key`

授权访问固定服务的密钥

值类型：`string`

## `Pubsub`

Pubsub用于配置`ipfs pubsub`子系统。 To use, it must be enabled by
passing the `--enable-pubsub-experiment` flag to the daemon.

### `Pubsub.Router`

设置pubsub默认使用的节点消息路由，可以是以下取值：
Sets the default router used by pubsub to route messages to peers. This can be one of:

- `"floodsub"` - floodsub是一个基础的路由，简单将消息发给所有连接的节点。此路由效率极低，但是非常可靠。
- `"gossipsub"` - [gossipsub][]是一个更高级的路由算法，它从网络链接的子集中构建了一层覆盖网格。

默认值：`"gossipsub"`

值类型：`string` (`"floodsub"`, `"gossipsub"`，或者`""`之一 (使用默认值))

[gossipsub](https://github.com/libp2p/specs/tree/master/pubsub/gossipsub)

### `Pubsub.DisableSigning`

禁用消息签名和签名验证。如果在完全可信的网络中运行节点，可以启用此选项。

即使不在意谁发送的消息，禁用签名依然不安全。因为通过恶意复用真实消息的消息ID，欺诈消息可以使得真实消息被静音忽略。

默认值：`false`

值类型：`bool`

## `Peering`

配置对等连接子系统。对等连接子系统配置了go-ipfs如何连接、保持连接和重新连接到一组节点。通过使用这个子系统，可以在常用的节点间创建粘性链接，从而提升可靠性。

使用场景：

- 连接到IPFS集群的IPFS网关应该保持节点链接，以确保网关总是可以从集群中获取内容。
- 一个dapp应该链接到内嵌的go-ipfs节点，从而可获取相应的内容固定服务或文件网关。
- 好友之间应该保持链接，以确保随时可以获取对方的内容信息。

当一个节点添加到互联节点集合中之后，go-ipfs会：

1. 在连接管理器中保持到该节点的连接。此时，go-ipfs永远不会自动断开到此节点的连接，而且与此节点的连接不会计入连接限制中。
3. 当节点离线或者之前的连接断开时，会反复尝试重连。重连逻辑按照随机指数衰退来计算重连间隔，时延从5s到10分钟进行变化，以避免一直反复重连已经离线的节点。

对等连接可以是非对称的或者对称的：

- 对称连接时，连接会被双方保护，从而保持稳定。
- 非对称连接时，只有一个节点（配置了对等连接的节点）会保护连接并在断开后尝试重连。当对等节点负载较重或者连接限制数很低时，连接可能反复波动。因此在使用非对称连接时，要注意保持负载不要多重。

### `Peering.Peers`

用于对等连接的节点集合。

```json
{
  "Peering": {
    "Peers": [
      {
        "ID": "QmPeerID1",
        "Addrs": ["/ip4/18.1.1.1/tcp/4001"]
      },
      {
        "ID": "QmPeerID2",
        "Addrs": ["/ip4/18.1.1.2/tcp/4001", "/ip4/18.1.1.2/udp/4001/quic"]
      }
    ]
  }
  ...
}
```

这里`ID`是节点的ID，`Addrs`是已知的节点地址集合。如果没有指定地址，将会查询DHT以获取。

未来可能扩展其他字段。

默认值：empty.

值类型：`array[peering]`

## `Reprovider`

### `Reprovider.Interval`

设置将本地内容重新提交到路由系统的时间间隔。如果未设置，默认值为12小时。如果设为`"0"`，将禁用内容重新提交。

注意：禁用内容重新提交将导致网络中的其他节点无法发现你拥有哪些数据内容。如果你既想禁用此选项，又期望网络能够知道你拥有的数据内容，需要手动定期向网络发布你所拥有的内容。

值类型：`array[peering]`

### `Reprovider.Strategy`

告知应该发布哪些重新提交内容。有效策略值包括：

- "all" - 发布所有存储的数据
- "pinned" - 只发布固定的数据
- "roots" - 只发布直接固定的键值，以及递归固定数据的根键值

默认值：all

值类型：`string` (未设置时取默认值)

## `Routing`

包含内容、节点和IPNS路由机制的选项。

### `Routing.Type`

内容路由模式。守护进程启动时的`--routing`选项会覆盖这里的取值。

有两种核心路由选项："none"和"dht" (默认)

- 设为"none"时，节点不会使用路由系统，必须明确连接到你需要的内容所在的节点，以获取其数据。
- 设为"dht"时(或者"dhtclient"/"dhtserver")，节点会使用IPFS DHT。

当启用DHT时，可以以两种模式运行：客户端 或者 服务器模式。

- 服务器模式下，节点会向其他节点查询DHT记录，同时也会响应其他节点的DHT请求（包括存储记录的请求和检索记录的请求）。
- 客户端模式下，节点会作为客户端向其他节点查询DHT，但不会响应其他节点的DHT请求。这种模式比服务器模式占用更少的资源。

当`Routing.Type`设为`dht`时，节点以DHT客户端模式启动,当检测到可被公网发现（如没有出于防火墙之后）时，将切换为DHT服务器模式。

也可以将`Routing.Type`设为`dhtclient`或`dhtserver`，以相应强制指定DHT为客户端或者服务器模式。如果无法确定你的节点可被公网发现，不要设置为`dhtserver`。

**示例：**

```json
{
  "Routing": {
    "Type": "dhtclient"
  }
}
```

默认值：dht

值类型：`string` (or unset for the default)

## `Swarm`

配置swarm的选项。

### `Swarm.AddrFilters`

一组不连接的地址（多重地址掩码）。默认情况下，IPFS节点会公告所有的地址，包括内部地址。这使得在同一网络内的节点更容易相互连接。不好的地方在于，这也意味着IPDS节点在连接其他节点时，会尝试连接一个或多个私有IP地址，即便其他节点处于与之不同的网络中。这可能会在某些托管服务商那里触发网络扫描的警告，或对某些设置造成压力。

`server`配置选项会为这个列表填充一个合理的默认值，阻止连接到所有不可路由的地址（如`192.168.0.0/16`），但是你总是应该把这些设置，和自己的网络环境或者托管服务商的网络环境对比检查。

默认值：`[]`

值类型：`array[string]`

### `Swarm.DisableBandwidthMetrics`

布尔值。设为true时，会导致IPFS不追踪带宽指标。禁用带宽指标可能会导致性能的轻微改善，同时减少了内存占用。

默认值：`false`

值类型：`bool`

### `Swarm.DisableNatPortMap`

禁用NAT端口自动转发。

没有禁用（默认值）时，go-ipfs会请求NAT设备（如路由器）打开一个外部端口，并将此端口的数据转发到go-ipfs运行的端口上。这种情况下（也就是你的路由器支持NAT端口转发），本地的go-ipfs节点可以被公网访问到。

默认值：`false`

值类型：`bool`

### `Swarm.DisableRelay`

已弃用。对应于设置`Swarm.Transports.Network.Relay`为`false`。

禁用p2p回环中继传输。这会禁止节点连接中继之后的节点，或者接受中继之后节点的连接。

默认值：`false`

值类型：`bool`

### `Swarm.EnableRelayHop`

配置本节点作为一个中继跳跃点。一个中继跳跃点能够为其他节点中继流量。

警告：除非你知道该行为的具体意义，否则不要启动此选项。其他节点可以随机决定使用你的节点作为中继，并消耗所有可用的带宽，且没有任何速率限制。

默认值：`false`

值类型：`bool`

### `Swarm.EnableAutoRelay`

启用本节点的自动中继模式。该选项与`Swarm.EnableRelayHop`一起会提供两种非常不同的特性。参考
[#7228](https://github.com/ipfs/go-ipfs/issues/7228)。

默认值：`false`

值类型：`bool`

#### 模式1： `EnableRelayHop`值为`false`

如果启用了`Swarm.EnableAutoRelay`，同时禁用了`Swarm.EnableRelayHop`，节点在发现自己无法被公网所发现时（如在防火墙之后），会自动使用网络中的公共中继。这是该选项所期望的特性。

如果启用了`EnableAutoRelay`，那么几乎可以确定应该禁用`EnableRelayHop`。

#### 模式2： `EnableRelayHop`值为`true`

如果启用了`Swarm.EnableAutoRelay`，同时还启用了`Swarm.EnableRelayHop`，节点将充当网络的公共中继。此外，除了简单的中继流量，节点还会对外发布自身为公共中继。除非你拥有小型ISP的带宽，否则不要同时启用这两个选项。

### `Swarm.EnableAutoNATService`

**已移除**

请使用 [`AutoNAT.ServiceMode`][].

### `Swarm.ConnMgr`

连接管理器用于决策哪些连接和多少连接可以被配置保持并保持这些连接。Go-ipfs当前支持两种连接管理器：

- none: 从不关闭空闲连接。
- basic: 默认的连接管理器。

默认值：basic

#### `Swarm.ConnMgr.Type`

设置要使用的连接管理器的类型，可选项为：`"none"` (无连接管理器)和`"basic"`。

默认值："basic".

值类型：`string` (未设置或值为`""`，使用默认的连接管理器，且忽略所有的`ConnMgr`其他字段值)。

#### basic连接管理器

基本连接管理器使用“高水位”、“低水位”和内部评分来定期关闭连接以回收资源。当一个节点使用了基本管理器，且空闲连接数达到“高水位”时，它将依次关闭最无用的连接，直到空闲连接数下降到“低水位”。

连接管理器在如下情形下判定一个连接为空闲状态：

- 它没有明确的收到某些子系统的保护。如Bitswap会保护其正在主动下载数据的节点连接。DHT会保护用于路由的节点。对等连接子系统会保护所有的对等节点连接。
- 该连接存活的时间超过了`GracePeriod`。

**Example:**

```json
{
  "Swarm": {
    "ConnMgr": {
      "Type": "basic",
      "LowWater": 100,
      "HighWater": 200,
      "GracePeriod": "30s"
    }
  }
}
```

##### `Swarm.ConnMgr.LowWater`

低水位是基本连接管理器会将连接数降低到的数值。

默认值：`600`

值类型：`integer`

##### `Swarm.ConnMgr.HighWater`

高水位是超过此连接数后，会触发连接垃圾回收的数值。注意：受保护的/最近形成的连接不会计算到这个限制统计中。

默认值：`900`

值类型：`integer`

##### `Swarm.ConnMgr.GracePeriod`

GracePeriod是一个时间长度值，表示免于被连接管理器关闭的新建连接的持续时长。

默认值：`"20s"`

值类型：`duration`

### `Swarm.Transports`

libp2p传输的配置部分。空配置则会使用默认值。

### `Swarm.Transports.Network`

libp2p网络传输的配置部分。这里启用的传输网络类型将用于对外连接，同时为了接受这些传输网络类型的连接，对应类型的多重地址需要添加到`Addresses.Swarm`选项中。

支持的传输类型包括：QUIC, TCP, WS, 和 Relay。

本节中的每个字段都是一个`flag`值。

#### `Swarm.Transports.Network.TCP`

[TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)是go-ipfs节点使用最广泛的传输协议。它本身并不直接支持加密或者多路复用，因此libp2p在其上构建了一个安全/多路复用的传输层。

默认值：Enabled

值类型：`flag`

监听地址：

- /ip4/0.0.0.0/tcp/4001 (默认)
- /ip6/::/tcp/4001 (默认)

#### `Swarm.Transports.Network.Websocket`

[Websocket](https://en.wikipedia.org/wiki/WebSocket)传输协议常用于基于浏览器的js-ipfs节点连接到非浏览器的IPFS节点的时候。

go-ipfs默认支持此传输协议的对外连接，但默认不会监听此传输类型。

默认值：Enabled

值类型：`flag`

监听地址：

- /ip4/0.0.0.0/tcp/4002/ws
- /ip6/::/tcp/4002/ws

#### `Swarm.Transports.Network.QUIC`

[QUIC](https://en.wikipedia.org/wiki/QUIC)是一个基于UDP的传输协议，且内建支持了加密和多路复用。它相对于TCP的优势在于：

1. 不需要每个连接占用一个文件描述符，减轻了操作系统的负载。
2. 目前只需要往返两次即可建立连接（TCP传输需要往返6次以完成连接）。

默认值：Enabled

值类型：`flag`

监听地址：

- /ip4/0.0.0.0/udp/4001/quic (默认)
- /ip6/::/udp/4001/quic (默认)

#### `Swarm.Transports.Network.Relay`

[Libp2p Relay](https://github.com/libp2p/specs/tree/master/relay)是一个代理传输协议，在多个libp2p节点间代理跳转以形成连接。这个传输协议在穿透防火墙和NATs时很有用。

默认值：Enabled

值类型：`flag`

监听地址：这个传输协议很特殊。任何启用此类型传输的节点无需指定监听地址，就能够收到此类型的传入连接。

### `Swarm.Transports.Security`

libp2p安全传输的配置部分。启用这部分特性能够增强未加密连接的安全性。

安全传输配置采用`priority`值类型。

建立出站连接的时候，go-ipfs会按优先级次序（从低到高）依次尝试每个安全传输选项，直到找到一个接收方也支持的协议为止。建立入站连接的时候，go-ipfs会让发起者选择协议，但会拒绝使用任何已禁用的传输协议。

支持的安全传输协议包括：TLS (priority 100), SECIO (禁用: 即priority false), Noise
(priority 300)。

任何默认的优先级都不会低于100。

#### `Swarm.Transports.Security.TLS`

[TLS](https://github.com/libp2p/specs/tree/master/tls) (1.3)是go-ipfs 0.5.0默认的安全传输协议。它也是最广泛被审查和可信的安全传输协议。

默认值：`100`

值类型：`priority`

#### `Swarm.Transports.Security.SECIO`

[SECIO](https://github.com/libp2p/specs/tree/master/secio) 曾是IPFS&libp2p安全传输层支持最广泛的协议。不过，目前正在被逐步淘汰，取而代之的是更流行的和经过更好的审查的协议，如TLS和Noise。

默认值：`false`

值类型：`priority`

#### `Swarm.Transports.Security.Noise`

[Noise](https://github.com/libp2p/specs/tree/master/noise) 因为其易于实现，将替代TLS作为跨平台libp2p的默认协议。当前默认启用，但因为尚未广泛支持，优先级较低。

默认值：`300`

值类型：`priority`

### `Swarm.Transports.Multiplexers`

libp2p多路复用传输的配置部分。启用这部分特性将支持多路复用的双工连接。

多路复用采用和安全传输一样的保护方式，使用`priority`值类型。与安全传输一样，发起者优先进行选择。

支持的传输类型包括：Yamux (priority 100) 和 Mplex (priority 200)

任何默认的优先级都不会低于100。

### `Swarm.Transports.Multiplexers.Yamux`

Yamux是go-ipfs节点间通讯时默认使用的多路复用器。

默认值：`100`

值类型：`priority`

### `Swarm.Transports.Multiplexers.Mplex`

Mplex是go-ipfs和所有其他节点以及libp2p实现通讯时默认使用的多路复用器。和Yamux不同的是：

The mounts config values specifies the default mount points for the IPFS and IPNS virtual file systems, if no other directories are specified by the `ipfs mount` command. These folders should exist, and have permissions for your user to be able to mount to them via fuse.

默认值：`200`

值类型：`priority`
