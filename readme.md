# hashicorp/go-getter [![explain]][source] [![translate-svg]][translate-list]

<!-- [![size-img]][size] -->

[explain]: http://llever.com/explain.svg
[source]: https://github.com/chinanf-boy/Source-Explain
[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list
[size-img]: https://packagephobia.now.sh/badge?p=Name
[size]: https://packagephobia.now.sh/result?p=Name

「 desc 」

[中文](./readme.md) | [english](https://github.com/hashicorp/go-getter)

---

## 校对 🀄️

<!-- doc-templite START generated -->
<!-- repo = 'hashicorp/go-getter' -->
<!-- commit = '7ee49f69de937ddc17eb72bc669c020b33c364d4' -->
<!-- time = '2018-11-13' -->

| 翻译的原文 | 与日期        | 最新更新 | 更多                       |
| ---------- | ------------- | -------- | -------------------------- |
| [commit]   | ⏰ 2018-11-13 | ![last]  | [中文翻译][translate-list] |

[last]: https://img.shields.io/github/last-commit/hashicorp/go-getter.svg
[commit]: https://github.com/hashicorp/go-getter/tree/7ee49f69de937ddc17eb72bc669c020b33c364d4

<!-- doc-templite END generated -->

### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)

## 生活

[help me live , live need money 💰](https://github.com/chinanf-boy/live-need-money)

---

### 目录

<!-- START doctoc -->
<!-- END doctoc -->

# 拼命三郎

[![Build Status](http://img.shields.io/travis/hashicorp/go-getter.svg?style=flat-square)][travis]
[![Build status](https://ci.appveyor.com/api/projects/status/ulq3qr43n62croyq/branch/master?svg=true)][appveyor]
[![Go Documentation](http://img.shields.io/badge/go-documentation-blue.svg?style=flat-square)][godocs]

[travis]: http://travis-ci.org/hashicorp/go-getter
[godocs]: http://godoc.org/github.com/hashicorp/go-getter
[appveyor]: https://ci.appveyor.com/project/hashicorp/go-getter/branch/master

go-getter 是 Go(golang)的一个库,它使用 URL 作为主要输入形式从各种来源下载文件或目录.

这个库的强大功能是能够使用单个字符串作为输入从许多不同的源(文件路径,Git,HTTP,Mercurial 等)下载.这消除了知道如何从实现者的各种来源下载的负担.

一个概念*探测器*自动将无效网址转换为正确的网址.例如:"github.com/hashicorp/go-getter"将变成 Git URL.或者"./foo"会变成文件 URL.这些都是可扩展的.

该库使用[Terraform](https://terraform.io)用于下载模块和[游牧](https://nomadproject.io)用于下载二进制文件.

## 安装和使用

可以在上找到包文档[GoDoc](http://godoc.org/github.com/hashicorp/go-getter).

安装可以正常进行`go get`:

```
$ go get github.com/hashicorp/go-getter
```

go-getter 还有一个可用于测试 URL 字符串的命令:

```
$ go install github.com/hashicorp/go-getter/cmd/go-getter
...

$ go-getter github.com/foo/bar ./foo
...
```

该命令对于验证 URL 结构很有用.

## 网址格式

go-getter 使用单个字符串 URL 作为输入从各种协议下载.go-getter 有这个 URL 的各种"技巧"来做某些事情.本节介绍了 URL 格式.

### 支持的协议和检测器

**协议**用于使用特定机制下载文件/目录.示例协议是 Git 和 HTTP.

**探测器**用于将有效或无效的 URL 转换为另一个 URL(如果它与特定模式匹配).示例:"github.com/user/repo"会自动转换为完全有效的 Git URL.这使得干将非常友好.

开箱即用的 go-getter 支持以下协议.通过实现,可以在运行时扩充其他协议`Getter`接口.

- 本地文件
- 混帐
- 水银
- HTTP
- 亚马逊 S3

除了上述协议,go-getter 还有所谓的"探测器".它们采用 URL 并尝试自动为其选择最佳协议,甚至可能涉及更改协议.

- 默认情况下内置以下检测:
- 诸如"./foo"之类的文件路径会自动更改为绝对文件 URL.
- GitHub URL,例如"github.com/mitchellh/vagrant"会自动更改为 HTTP 上的 Git 协议.

### BitBucket URL,例如"bitbucket.org/mitchellh/vagrant",会使用 BitBucket API 自动更改为 Git 或 mercurial 协议.

强制协议在某些情况下,根据源 URL,使用的协议不明确.[例如, "](http://github.com/mitchellh/vagrant.git)http\://github.com/mitchellh/vagrant.git"可以引用 HTTP URL 或 Git URL.强制协议语法用于消除此 URL 的歧义.

强制协议可以通过在 URL 前面加上协议后跟双冒号来完成.例如:`git::http://github.com/mitchellh/vagrant.git`将使用 Git 协议下载给定的 HTTP URL.

强制协议也将覆盖任何检测器.

在没有强制协议的情况下,可以在 URL 上运行检测器,无论如何都要转换协议.以上示例将使用 Git 协议,因为 Git 检测器会检测到它是 GitHub URL.

### 特定于协议的选项

每个协议都可以支持特定于协议的选项来配置该协议.例如,`git`协议支持指定`ref`查询参数,告诉它什么引用该 Git 存储库的结帐.

这些选项被指定为给予 go-getter 的 URL(或类似 URL 的字符串)的查询参数.使用上面的 Git 示例,下面的 URL 是 go-getter 的有效输入:

```
github.com/hashicorp/go-getter?ref=abcd1234
```

特定于协议的选项记录在 URL 格式部分下面.但由于它们是 URL 的一部分,我们在此指出它,因此您知道它们存在.

### 子目录

如果只想从下载的目录下载特定的子目录,可以在双斜杠后指定子目录`//`.go-getter 将首先下载指定的 URL*之前*双斜杠(如果没有指定双斜杠),但会将双斜杠后的路径复制到目标目录中.

例如,如果您正在下载此 GitHub 存储库,但您只想下载`test-fixtures`目录,您可以执行以下操作:

```
https://github.com/hashicorp/go-getter.git//test-fixtures
```

如果你下载到了`/tmp`目录,然后是文件`/tmp/archive.gz`会存在.请注意,此文件位于`test-fixtures`此存储库中的目录,但由于我们指定了一个子目录,因此 go-getter 仅自动复制该目录内容.

子目录路径可能包含也可能使用文件系统 glob 模式.路径必须匹配*一个*entry 或 go-getter 将返回错误.如果您不确定确切的目录名称,但它遵循可预测的命名结构,这将非常有用.

例如,以下 URL 也可以使用:

```
https://github.com/hashicorp/go-getter.git//test-*
```

### 检查校验

对于任何协议的文件下载,go-getter 可以自动为您验证校验和.请注意,校验和仅适用于下载文件,而不适用于下载目录,但校验和将适用于任何协议.

要校验文件,请附加一个`checksum`查询 URL 的参数.参数值应采用格式`type:value`,其中 type 是"md5","sha1","sha256"或"sha512"."值"应该是实际的校验和值.go-getter 会自动解析出这个查询参数,并用它来验证校验和.示例网址如下所示:

```
./foo.txt?checksum=md5:b7d96c89d09d9e204f5fedc4d5d55b21
```

校验和查询参数永远不会发送到后端协议实现.它由 go-getter 本身在更高层次上使用.

### 解除封存

go-getter 将根据所请求文件的扩展名(通过任何协议)自动将文件解压缩到文件或目录中.这适用于文件和目录下载.

go-getter 寻找一个`archive`query 参数指定存档的格式.如果未指定,则 go-getter 将使用路径的扩展名来查看它是否已存档.可以通过设置显式禁用取消归档`archive`查询参数`false`.

支持以下存档格式:

- `tar.gz`和`tgz`
- `tar.bz2`和`tbz2`
- `tar.xz`和`txz`
- `zip`
- `gz`
- `bz2`
- `xz`

例如,示例 URL 如下所示:

```
./foo.zip
```

这将自动推断为 ZIP 文件并将被提取.您还可以明确说明存档类型:

```
./some/other/path?archive=zip
```

最后,您可以完全禁用存档:

```
./some/path?archive=false
```

您可以将 unarchiving 与 go-getter 的其他功能(例如校验和)结合使用.特别的`archive`在进入最终协议下载程序之前,将从 URL 中删除查询参数.

## 特定于协议的选项

本节介绍了可以为 go-getter 指定的特定于协议的选项.这些选项应作为普通查询参数附加到输入中([HTTP 标头](#headers)然而,这是一个例外.根据使用者的使用情况,应用程序可以提供输入选项的替代方法.例如,[游牧](https://www.nomadproject.io)提供了一个很好的选项块来指定选项而不是 URL.

## 一般(所有议定书)

以下选项适用于所有协议:

- `archive`- 用于取消归档此文件的归档格式,或""(空字符串)以禁用取消归档.有关更多详细信息,请参阅上面有关存档支持的完整部分.

- `checksum`- 校验和以验证下载的文件或存档.有关格式和更多详细信息,请参阅上面有关校验和的整个部分.

- `filename`- 在文件下载模式下,允许在磁盘上指定下载文件的名称.在目录模式下无效.

### 本地文件(`file`)

没有

### Git(`git`)

- `ref`- Git 参考结帐.这是一个 ref,所以它可以指向提交 SHA,分支名称等.如果它是一个名为 ref 的分支名称,那么 go-getter 会在每次获取时将其更新为最新版本.

- `sshkey`- 克隆期间使用的 SSH 私钥.提供的密钥必须是 base64 编码的字符串.例如,生成合适的`sshkey`从磁盘上的私钥文件,您将运行`base64 -w0 <file>`.

  **注意**:使用此功能需要 Git 2.3+.

### Mercurial(`hg`)

- `rev`- 结账的 Mercurial 修订版.

### HTTP(`http`)

#### 基本认证

要使用 go-getter 进行 HTTP 基本身份验证,只需预先添加`username:password@`到 URL 中的主机名如`https://Aladdin:OpenSesame@www.example.com/index.html`.所有特殊字符(包括用户名和密码)都必须进行 URL 编码.

#### 头

可以通过自定义提供可选请求标头来添加它们[`HttpGetter`](https://godoc.org/github.com/hashicorp/go-getter#HttpGetter)(*不*像大多数其他选项一样的查询参数).这些标题将在有问题的 getter 的每个请求中发送出去.

### S3(`s3`)

S3 采用 URL 中的各种访问配置.请注意,如果已设置,它还将从标准 AWS 环境变量中读取这些内容.还支持像 Minio 这样的 S3 兼容服务器.如果查询参数存在,则优先考虑这些参数.

- `aws_access_key_id`- AWS 访问密钥.
- `aws_access_key_secret`- AWS 访问密钥秘密.
- `aws_access_token`- AWS 访问令牌(如果正在使用).

#### 在 S3 中使用 IAM 实例配置文件

如果您使用 go-getter 并希望使用 EC2 IAM 实例配置文件以避免使用凭据,则只需省略这些内容即可自动使用配置文件(如果可用).

### 将 S3 与 Minio 一起使用

如果您使用 the-gitter 来支持 Minio,则必须考虑以下事项:

- `aws_access_key_id`(必填) - Minio 访问密钥.
- `aws_access_key_secret`(必填) - Minio 访问密钥秘密.
- `region`(可选 - 默认为 us-east-1) - 要使用的区域标识符.
- `version`(可选 - 默认为 Minio 默认值) - 配置文件格式.

#### S3 Bucket 示例

S3 有几种用于引用您的存储桶的寻址方案.这些列在这里:<http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html#access-bucket-intro>

这些寻址方案的一些例子:

- S3 ::<https://s3.amazonaws.com/bucket/foo>
- S3 ::<https://s3-eu-west-1.amazonaws.com/bucket/foo>
- bucket.s3.amazonaws.com/foo
- bucket.s3-eu-west-1.amazonaws.com/foo/bar
- "S3 ::[http://127.0.0.1:9000/test-bucket/hello.txt?aws_access_key_id=KEYID&aws_access_key_secret=SECRETKEY® 离子= ​​US-东-2-](http://127.0.0.1:9000/test-bucket/hello.txt?aws_access_key_id=KEYID&aws_access_key_secret=SECRETKEY&region=us-east-2)"
