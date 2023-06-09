---
title: Hexo配置参数
date: 2023-06-01 21:45:41
categories:
  - Hexo
tags:
  - Hexo
---

## 建站

<!-- more -->

安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

```
$ hexo init <folder>
$ cd <folder>
$ npm install
```

新建完成后，指定文件夹的目录如下：

```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

**_config.yml** 

网站的 **配置** 信息，您可以在此配置大部分的参数。

**package.json**

应用程序的信息。EJS, Stylus 和 Markdown renderer 已默认安装，您可以自由移除。

```
package.json
{
  "name": "hexo-site",
  "version": "0.0.0",
  "private": true,
  "hexo": {
    "version": ""
  },
  "dependencies": {
    "hexo": "^3.8.0",
    "hexo-generator-archive": "^0.1.5",
    "hexo-generator-category": "^0.1.3",
    "hexo-generator-index": "^0.2.1",
    "hexo-generator-tag": "^0.2.0",
    "hexo-renderer-ejs": "^0.3.1",
    "hexo-renderer-stylus": "^0.3.3",
    "hexo-renderer-marked": "^0.3.2",
    "hexo-server": "^0.3.3"
  }
}
```

**scaffolds**

模版 文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件。

Hexo的模板是指在新建的文章文件中默认填充的内容。例如，如果您修改scaffold/post.md中的Front-matter内容，那么每次新建一篇文章时都会包含这个修改。

**source**

资源文件夹是存放用户资源的地方。除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。

**themes**

主题 文件夹。Hexo 会根据主题来生成静态页面。

## 网站
|参数	|描述|
|-----------|---------------------------------------------------|
|title	|网站标题|
|subtitle	|网站副标题|
|description	|网站描述|
|keywords	|网站的关键词。支持多个关键词。|
|author	|您的名字|
|language	|网站使用的语言。对于简体中文用户来说，使用不同的主题可能需要设置成不同的值，请参考你的主题的文档自行设置，常见的有 zh-Hans和 zh-CN。|
|timezone	|网站时区。Hexo 默认使用您电脑的时区。请参考 时区列表 进行设置，如 America/New_York, Japan, 和 UTC 。一般的，对于中国大陆地区可以使用 Asia/Shanghai。|

其中，description主要用于SEO，告诉搜索引擎一个关于您站点的简单描述，通常建议在其中包含您网站的关键词。author参数用于主题显示文章的作者。

## 网址
|参数	|描述	|默认值|
|---------------------------|-----------------------------|--------|
|url	|网址, 必须以 http:// 或 https:// 开头	||
|root	|网站根目录	|url's pathname|
|permalink	|文章的 永久链接 格式	|:year/:month/:day/:title/
|permalink_defaults	|永久链接中各部分的默认值	||
|pretty_urls	|改写 permalink 的值来美化 |URL	|
|pretty_urls.trailing_index	|是否在永久链接中保留尾部的 index.html，设置为 false 时去除	|true|
|pretty_urls.trailing_html	|是否在永久链接中保留尾部的 .html, 设置为 false 时去除 (对尾部的 index.html无效)	|true|

网站存放在子目录

如果您的网站存放在子目录中，例如 http://example.com/blog，则请将您的 url 设为 http://example.com/blog 并把 root 设为 /blog/。

例如：

```
#比如，一个页面的永久链接是 http://example.com/foo/bar/index.html
pretty_urls:
  trailing_index: false
#此时页面的永久链接会变为 http://example.com/foo/bar/
```

## 目录
|参数	|描述|默认值
|-------------------|------------------------------------|----------------|
|source_dir	|资源文件夹，这个文件夹用来存放内容。	|source|
|public_dir	|公共文件夹，这个文件夹用于存放生成的站点文件。	|public|
|tag_dir	|标签文件夹	|tags|
|archive_dir	|归档文件夹	|archives|
|category_dir	|分类文件夹	|categories|
|code_dir|Include code 文件夹，source_dir 下的子目录	|downloads/code|
|i18n_dir	|国际化（i18n）文件夹	|:lang|
|skip_render	|跳过指定文件的渲染。匹配到的文件将会被不做改动地复制到 public 目录中。您可使用 glob 表达式来匹配路径。|	

例如：

```
skip_render: "mypage/**/*"
# 将会直接将 `source/mypage/index.html` 和 `source/mypage/code.js` 不做改动地输出到 'public' 目录
# 你也可以用这种方法来跳过对指定文章文件的渲染
skip_render: "_posts/test-post.md"
# 这将会忽略对 'test-post.md' 的渲染
```
##### 提示
如果您刚刚开始接触 Hexo，通常没有必要修改这一部分的值。

## 文章
|参数	|描述	|默认值|
|--------------------|-------------------------------------|----------|
|new_post_name	|新文章的文件名称	|:title.md|
|default_layout	|预设布局	|post|
|auto_spacing	|在中文和英文之间加入空格	|false|
|titlecase	|把标题转换为 title case	|false|
|external_link	|在新标签中打开链接	|true|
|external_link.enable	|在新标签中打开链接	|true|
|external_link.field	|对整个网站（site）生效或仅对文章（post）生效	|site|
|external_link.exclude	|需要排除的域名。主域名和子域名如 www 需分别配置	|[]|
|filename_case	|把文件名称转换为 (1) 小写或 (2) 大写	|0|
|render_drafts	|显示草稿	|false|
|post_asset_folder	|启动 Asset 文件夹	|false|
|relative_link	|把链接改为与根目录的相对位址	|false|
|future	|显示未来的文章	|true|
|highlight	|代码块的设置, 请参考 Highlight.js 进行设置	| |
|prismjs	|代码块的设置, 请参考 PrismJS 进行设置	| |

**相对地址**

默认情况下，Hexo 生成的超链接都是绝对地址。例如，如果您的网站域名为 example.com，您有一篇文章名为 hello，那么绝对链接可能像这样：http://example.com/hello.html，它是绝对于域名的。相对链接像这样：/hello.html，也就是说，无论用什么域名访问该站点，都没有关系，这在进行反向代理时可能用到。通常情况下，建议使用绝对地址。

|分类 |& |标签|
|----------------|--------|---------------|
|参数	|描述	|默认值|
|default_category	|默认分类	|uncategorized|
|category_map	|分类别名	|
|tag_map	|标签别名	|

**日期 / 时间格式**

Hexo 使用 Moment.js 来解析和显示时间。

|参数	|描述	|默认值|
|----------------|--------|---------------|
|date_format	|日期格式	|YYYY-MM-DD|
|time_format	|时间格式	|HH:mm:ss|
|updated_option	|当 Front Matter 中没有指定 updated 时 updated 的取值	|mtime|

**updated_option**

updated_option 控制了当 Front Matter 中没有指定 updated 时，updated 如何取值：

mtime: 使用文件的最后修改时间。这是从 Hexo 3.0.0 开始的默认行为。

date: 使用 date 作为 updated 的值。可被用于 Git 工作流之中，因为使用 Git 管理站点时，文件的最后修改日期常常会发生改变

empty: 直接删除 updated。使用这一选项可能会导致大部分主题和插件无法正常工作。

use_date_for_updated 选项已经被废弃，将会在下个重大版本发布时去除。请改为使用 updated_option: 'date'。

### 分页
|参数	|描述	|默认值|
|----------------|--------|---------------|
|per_page	|每页显示的文章量 (0 = 关闭分页功能)	|10|
|pagination_dir	|分页目录	|page|

### 扩展

|参数|	描述|
|---------------|---------------------------------------|
|theme	|当前主题名称。值为false时禁用主题|
|theme_config	|主题的配置文件。在这里放置的配置会覆盖主题目录下的 _config.yml 中的配置|
|deploy	|部署部分的设置|

**meta_generator**	

Meta generator 标签。 值为 false 时 Hexo 不会在头部插入该标签

**包括或不包括目录和文件**

在 Hexo 配置文件中，通过设置 include/exclude 可以让 Hexo 进行处理或忽略某些目录和文件夹。你可以使用 glob 表达式 对目录和文件进行匹配。

include 和 exclude 选项只会应用到 source/ ，而 ignore 选项会应用到所有文件夹.

|参数	|描述|
|include	|Hexo 默认会不包括 source/ 下的文件和文件夹（包括名称以下划线和 . 开头的文件和文件夹，Hexo 的 _posts 和 _data 等目录除外）。通过设置此字段将使 Hexo 处理他们并将它们复制到 source 目录下。|
|exclude	|Hexo 不包括 source/ 下的这些文件和目录|
|ignore	|Hexo 会忽略整个 Hexo 项目下的这些文件夹或文件|

举例：

#处理或不处理目录或文件

```
include:
  - ".nojekyll"
  # 处理 'source/css/_typing.css'
  - "css/_typing.css"
  # 处理 'source/_css/' 中的任何文件，但不包括子目录及其其中的文件。
  - "_css/*"
  # 处理 'source/_css/' 中的任何文件和子目录下的任何文件
  - "_css/**/*"

exclude:
  # 不处理 'source/js/test.js'
  - "js/test.js"
  # 不处理 'source/js/' 中的文件、但包括子目录下的所有目录和文件
  - "js/*"
  # 不处理 'source/js/' 中的文件和子目录下的任何文件
  - "js/**/*"
  # 不处理 'source/js/' 目录下的所有文件名以 'test' 开头的文件，但包括其它文件和子目录下的单文件
  - "js/test*"
  # 不处理 'source/js/' 及其子目录中任何以 'test' 开头的文件
  - "js/**/test*"
  # 不要用 exclude 来忽略 'source/_posts/' 中的文件。你应该使用 'skip_render'，或者在要忽略的文件的文件名之前加一个下划线 '_'
  # 在这里配置一个 - "_posts/hello-world.md" 是没有用的。

ignore:
  # 忽略任何一个名叫 'foo' 的文件夹
  - "**/foo"
  # 只忽略 'themes/' 下的 'foo' 文件夹
  - "**/themes/*/foo"
  # 对 'themes/' 目录下的每个文件夹中忽略名叫 'foo' 的子文件夹
  - "**/themes/**/foo"
```

列表中的每一项都必须用单引号或双引号包裹起来。

include 和 exclude 并不适用于 themes/ 目录下的文件。如果需要忽略 themes/ 目录下的部分文件或文件夹，可以使用 ignore 或在文件名之前添加下划线 _。

**使用代替配置文件**

可以在 hexo-cli 中使用 --config 参数来指定自定义配置文件的路径。你可以使用一个 YAML 或 JSON 文件的路径，也可以使用逗号分隔（无空格）的多个 YAML 或 JSON 文件的路径。例如：

**#用 'custom.yml' 代替 '_config.yml'**

```
  $ hexo server --config custom.yml
```

**#使用 'custom.yml' 和 'custom2.json'，优先使用 'custom3.yml'，然后是 'custom2.json'
**
```
  $ hexo generate --config custom.yml,custom2.json,custom3.yml
```

当你指定了多个配置文件以后，Hexo 会按顺序将这部分配置文件合并成一个 _multiconfig.yml。如果遇到重复的配置，排在后面的文件的配置会覆盖排在前面的文件的配置。这个原则适用于任意数量、任意深度的 YAML 和 JSON 文件。

例如，使用 --options 指定了两个自定义配置文件：

```
  $ hexo generate --config custom.yml,custom2.json
```

如果 custom.yml 中指定了 foo: bar，在 custom2.json 中指定了 "foo": "dinosaur"，那么在 _multiconfig.yml 中你会得到 foo: dinosaur。

使用代替主题配置文件

通常情况下，Hexo 主题是一个独立的项目，并拥有一个独立的 _config.yml 配置文件。

除了自行维护独立的主题配置文件，你也可以在其它地方对主题进行配置。

配置文件中的 theme_config

该特性自 Hexo 2.8.2 起提供

```
#_config.yml
theme: "my-theme"
theme_config:
  bio: "My awesome bio"
  foo:
    bar: 'a'
#themes/my-theme/_config.yml
bio: "Some generic bio"
logo: "a-cool-image.png"
  foo:
    baz: 'b'
```

最终主题配置的输出是：

```
{
  bio: "My awesome bio",
  logo: "a-cool-image.png",
  foo: {
    bar: "a",
    baz: "b"
  }
}
```

独立的 **_config.[theme].yml** 文件

该特性自 Hexo 5.0.0 起提供

独立的主题配置文件应放置于站点根目录下，支持 yml 或 json 格式。需要配置站点 **_config.yml** 文件中的 theme 以供 Hexo 寻找 **_config.[theme].yml** 文件。

```
#_config.yml
theme: "my-theme"
#_config.my-theme.yml
bio: "My awesome bio"
foo:
  bar: 'a'
#themes/my-theme/_config.yml
bio: "Some generic bio"
logo: "a-cool-image.png"
  foo:
    baz: 'b'
```

最终主题配置的输出是：

```
{
  bio: "My awesome bio",
  logo: "a-cool-image.png",
  foo: {
    bar: "a",
    baz: "b"
  }
}
```

我们强烈建议你将所有的主题配置集中在一处。如果你不得不在多处配置你的主题，那么这些信息对你将会非常有用：Hexo 在合并主题配置时，Hexo 配置文件中的 theme_config 的优先级最高，其次是 _config.[theme].yml 文件，最后是位于主题目录下的 _config.yml 文件。

### 指令

**init**

```
$ hexo init [folder]
```

新建一个网站。如果没有设置 folder ，Hexo 默认在目前的文件夹建立网站。

本命令相当于执行了以下几步：

Git clone hexo-starter 和 hexo-theme-landscape 主题到当前目录或指定目录。

使用 Yarn 1、pnpm 或 npm 包管理器下载依赖（如有已安装多个，则列在前面的优先）。npm 默认随 Node.js 安装。

**new**

```
$ hexo new [layout] <title>
```

新建一篇文章。如果没有设置 layout 的话，默认使用 _config.yml 中的 default_layout 参数代替。如果标题包含空格的话，请使用引号括起来。

```
$ hexo new "post title with whitespace"
```

|参数	|描述|
|--------------|-------------------------------------|
|-p, --path	|自定义新文章的路径|
|-r, --replace	|如果存在同名文章，将其替换|
|-s, --slug	|文章的 Slug，作为新文章的文件名和发布后的 URL|

默认情况下，Hexo 会使用文章的标题来决定文章文件的路径。对于独立页面来说，Hexo 会创建一个以标题为名字的目录，并在目录中放置一个 index.md 文件。你可以使用 --path 参数来覆盖上述行为、自行决定文件的目录：

```
hexo new page --path about/me "About me"
```

以上命令会创建一个 source/about/me.md 文件，同时 Front Matter 中的 title 为 "About me"

注意！title 是必须指定的！如果你这么做并不能达到你的目的：

```
hexo new page --path about/me
```

此时 Hexo 会创建 source/_posts/about/me.md，同时 me.md 的 Front Matter 中的 title 为 "page"。这是因为在上述命令中，hexo-cli 将 page 视为指定文章的标题、并采用默认的 layout。

**generate**

```
$ hexo generate
```

生成静态文件。

|选项	|描述|
|---------------|---------------------------------|
|-d, --deploy	|文件生成后立即部署网站|
|-w, --watch	|监视文件变动|
|-b, --bail	|生成过程中如果发生任何未处理的异常则抛出异常|
|-f, --force	|强制重新生成文件|
|-c, --concurrency	|最大同时生成文件的数量，默认无限制|

Hexo 引入了差分机制，如果 public 目录存在，那么 hexo g 只会重新生成改动的文件。

使用该参数的效果接近 ```hexo clean && hexo generate```

**-c, --concurrency**	最大同时生成文件的数量，默认无限制
该命令可以简写为

```
$ hexo g
publish
$ hexo publish [layout] <filename>
```

发表草稿。

**server**

```
$ hexo server
```

启动服务器。默认情况下，访问网址为： http://localhost:4000/。

|选项	|描述|
|-------------|----------------------|
|-p, --port	|重设端口|
|-s, --static	|只使用静态文件|
|-l, --log	|启动日记记录，使用覆盖记录格式|

**deploy**

```
$ hexo deploy
```

部署网站。

|参数	|描述|
|---------------|--------------------|
|-g, --generate	|部署之前预先生成静态文件|

该命令可以简写为：

```
$ hexo d
```

**render**

```
$ hexo render <file1> [file2] ...
```

渲染文件。

|参数	|描述|
|-------------|-----------|
|-o, --output	|设置输出路径|

**migrate**

```
$ hexo migrate <type>
```

从其他博客系统 迁移内容。

**clean**

```
$ hexo clean
```

清除缓存文件 (db.json) 和已生成的静态文件 (public)。

在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。

**list**

```
$ hexo list <type>
```

列出网站资料。

**version**

```
$ hexo version
```

显示 Hexo 版本。

## 选项

### 安全模式

```
$ hexo --safe
```

在安全模式下，不会载入插件和脚本。当您在安装新插件遭遇问题时，可以尝试以安全模式重新执行。

### 调试模式

```
$ hexo --debug
```

在终端中显示调试信息并记录到 debug.log。当您碰到问题时，可以尝试用调试模式重新执行一次，并 提交调试信息到 GitHub。

### 简洁模式

```
$ hexo --silent
```

隐藏终端信息。

自定义配置文件的路径

**#使用 custom.yml 代替默认的 _config.yml**

```
$ hexo server --config custom.yml
```

**#使用 custom.yml 和 custom2.json，其中 custom2.json 优先级更高**

```
$ hexo generate --config custom.yml,custom2.json,custom3.yml
```

自定义配置文件的路径，指定这个参数后将不再使用默认的 _config.yml。

你可以使用一个 YAML 或 JSON 文件的路径，也可以使用逗号分隔（无空格）的多个 YAML 或 JSON 文件的路径。例如：

**#使用 custom.yml 代替默认的 _config.yml**

```
$ hexo server --config custom.yml
```

**#使用 custom.yml, custom2.json 和 custom3.yml，其中 custom3.yml 优先级最高，其次是 custom2.json**

```
$ hexo generate --config custom.yml,custom2.json,custom3.yml
```

当你指定了多个配置文件以后，Hexo 会按顺序将这部分配置文件合并成一个 _multiconfig.yml。如果遇到重复的配置，排在后面的文件的配置会覆盖排在前面的文件的配置。这个原则适用于任意数量、任意深度的 YAML 和 JSON 文件。

### 显示草稿

```
$ hexo --draft
```

显示 source/_drafts 文件夹中的草稿文章。

### 自定义 CWD

```
$ hexo --cwd /path/to/cwd
```

自定义当前工作目录（Current working directory）的路径。

## 迁移

### RSS

首先，安装 hexo-migrator-rss 插件。

```
$ npm install hexo-migrator-rss --save
```

插件安装完成后，执行下列命令，从 RSS 迁移所有文章。source 可以是文件路径或网址。

```
$ hexo migrate rss <source>
```

### Jekyll

把 _posts 文件夹内的所有文件复制到 source/_posts 文件夹，并在 _config.yml 中修改 new_post_name 参数。

**new_post_name: :year-:month-:day-:title.md**

### Octopress

把 Octopress source/_posts 文件夹内的所有文件转移到 Hexo 的 source/_posts 文件夹，并修改 _config.yml 中的 new_post_name 参数。

**new_post_name: :year-:month-:day-:title.md**

### WordPress

首先，安装 hexo-migrator-wordpress 插件。

```
$ npm install hexo-migrator-wordpress --save
```

在 WordPress 仪表盘中导出数据(“Tools” → “Export” → “WordPress”)（详情参考WP支持页面）。

插件安装完成后，执行下列命令来迁移所有文章。source 可以是 WordPress 导出的文件路径或网址。

```
$ hexo migrate wordpress <source>
```

**注意**

这个插件并不能完美地实现WordPress->Hexo的数据转换，尤其是在处理WordPress的分类方面存在问题（见Front-matter中的分类与标签）。因此，建议您在迁移完成后，手工审阅所有生成的markdown文件，检查其中是否有错误。对于文章数量较大的WordPress站点，这项工作可能要花很长的时间。

### Joomla

首先，安装 hexo-migrator-joomla 插件。

```
$ npm install hexo-migrator-joomla --save
```

使用 J2XML 组件导出 Joomla 文章。

插件安装完成后，执行下列命令来迁移所有文章。source 可以是 Joomla 导出的文件路径或网址。

```
$ hexo migrate joomla <source>
```

## 写作

你可以执行下列命令来创建一篇新文章或者新的页面。

```
$ hexo new [layout] <title>
```

您可以在命令中指定文章的布局（layout），默认为 post，可以通过修改 _config.yml 中的 default_layout 参数来指定默认布局。

### 布局（Layout）

Hexo 有三种默认布局：post、page 和 draft。在创建这三种不同类型的文件时，它们将会被保存到不同的路径；而您自定义的其他布局和 post 相同，都将储存到 source/_posts 文件夹。

|布局	|路径|
|---------|-----------|
|post	|source/_posts|
|page	|source|
|draft	|source/_drafts|

### 禁用布局

如果你不希望一篇文章（post/page）使用主题处理，请在它的 front-matter 中设置 layout: false。详情请参考本节。

### 文件名称

Hexo 默认以标题做为文件名称，但您可编辑 new_post_name 参数来改变默认的文件名称，举例来说，设为 :year-:month-:day-:title.md 可让您更方便的通过日期来管理文章。

|变量	|描述|
|-----------|-------------------------|
|:title	|标题（小写，空格将会被替换为短杠）|
|:year	|建立的年份，比如， 2015|
|:month	|建立的月份（有前导零），比如， 04|
|:i_month	|建立的月份（无前导零），比如， 4|
|:day	|建立的日期（有前导零），比如， 07|
|:i_day	|建立的日期（无前导零），比如， 7|

### 草稿

刚刚提到了 Hexo 的一种特殊布局：draft，这种布局在建立时会被保存到 source/_drafts 文件夹，您可通过 publish 命令将草稿移动到 source/_posts 文件夹，该命令的使用方式与 new 十分类似，您也可在命令中指定 layout 来指定布局。

```
$ hexo publish [layout] <title>
```

草稿默认不会显示在页面中，您可在执行时加上 --draft 参数，或是把 render_drafts 参数设为 true 来预览草稿。

### 模版（Scaffold）

在新建文章时，Hexo 会根据 scaffolds 文件夹内相对应的文件来建立文件，例如：

```
$ hexo new photo "My Gallery"
```

在执行这行指令时，Hexo 会尝试在 scaffolds 文件夹中寻找 photo.md，并根据其内容建立文章，以下是您可以在模版中使用的变量：

|变量	|描述|
|------------|--------------|
|layout	|布局|
|title	|标题|
|date	|文件建立日期|

### 支持的格式

Hexo 支持以任何格式书写文章，只要安装了相应的渲染插件。

例如，Hexo 默认安装了 hexo-renderer-marked 和 hexo-renderer-ejs，因此你不仅可以用 Markdown 写作，你还可以用 EJS 写作。如果你安装了 hexo-renderer-pug，你甚至可以用 Pug 模板语言书写文章。

只需要将文章的扩展名从 md 改成 ejs，Hexo 就会使用 hexo-renderer-ejs 渲染这个文件，其他格式同理。
