## 1	查什么(what)

![[Pasted image 20240629032610.png]]
### 1.1	隐式字段 

#### 1.1.1	 file.文件隐式字段

- `file.name`: 文件标题（一个字符串）.
- `file.folder`: 这个文件所属的文件夹的路径.
- `file.path`: 完整的文件路径（一个字符串）.
- `file.ext`: 文件类型的扩展名；一般为'.md'（字符串）.
- `file.link`: 通往该文件的链接（一个链接）
- `file.size`: 文件的大小（以字节为单位）（一个数字）.
- `file.ctime`: 该文件的创建日期（一个日期+时间）
- `file.cday`: 文件创建的日期（只是一个日期）.
- `file.mtime`: 文件最后被修改的日期（一个日期+时间）。.
- `file.mday`: 文件最后被修改的日期（只是一个日期）。.
- `file.tags`: 笔记中所有独特标签的一个数组. 小标签按每个级别进行细分, so `#Tag/1/A` 将被存储在数组中，作为 `[#Tag, #Tag/1, #Tag/1/A]`.
- `file.etags`: 注释中所有显式标签的数组; unlike `file.tags`, 不包括子标签.
- `file.inlinks`: 该文件的所有传入链接的数组.
- `file.outlinks`: 该文件中所有外链的数组.
- `file.aliases`: 笔记中所有别名的一个数组.
- `file.tasks`: 一个包含所有任务的数组 (I.e., `- [ ] blah blah blah`) 在这个文件中。
- `file.lists`: 文件中所有列表元素的数组（包括任务）；这些元素是有效的任务，可以在任务视图中呈现。.
- `file.frontmatter`: 包含所有frontmatter的原始值；主要用于检查frontmatter的原始值或动态地列出前题的 keys 键值(字段)。

如果文件的标题内有一个日期（形式为 `yyyy-mm-dd` 或 `yyyymmdd`），或有一个Date字段/inline字段，它也有以下属性。

- `file.day`: 与文件标题相关的明确日期. 如果你使用obsidian核心插件 "Starred Files"，以下元数据也是可用的。
- `file.starred`: 如果这个文件已经被 "stars " obsidian插件加了星号。

#### 1.1.2	task.待办隐式字段

- `status`: 该任务的完成状态，由`[]`括号内的字符决定。一般来说，空格`" "`表示未完成的任务，X`"X"`表示完成的任务，但允许支持其他任务状态的插件。
- `checked`: 该任务是否以任何方式被检查过（即它的状态不是未完成/空的）。.
- `completed`: 这个_具体的_任务是否已经完成；这不考虑任何子任务的完成/未完成情况。如果一项任务被标记为 "X"，则明确视为 "完成"。.
- `fullyCompleted`: Whether or not this task and all of its subtasks are completed.
- `text`: 这项任务的文本.
- `line`: 该任务显示的行.
- `lineCount`: 这项任务所占的 markdown 行数.
- `path`: 该任务所在文件的完整路径.
- `section`: 该任务包含的章节链接.
- `tags`: 文本任务内的任何标签。
- `outlinks`: 本任务中定义的任何链接.
- `link`: 通往该任务附近最近的可链接区块的链接；对于建立通往该任务的链接很有用。.
- `children`: 该任务的任何子任务或子清单.
- `task`: 如果为真，这是一个任务；否则，它是一个普通的列表元素.
- `completion`: 一项任务完成的日期; set by `completion...` or [shorthand syntaxopen in new window](https://blacksmithgu.github.io/obsidian-dataview/annotation/metadata-tasks/#field-shorthands).
- `due`: 任务到期的日期，如果它有一个。. Set by `due...` or [shorthand syntaxopen in new window](https://blacksmithgu.github.io/obsidian-dataview/annotation/metadata-tasks/#field-shorthands).
- `created`: 一个任务的创建日期; set by `created...` or [shorthand syntaxopen in new window](https://blacksmithgu.github.io/obsidian-dataview/annotation/metadata-tasks/#field-shorthands).
- `start`: 一个任务可以开始的日期; set by `start...` or [shorthand syntaxopen in new window](https://blacksmithgu.github.io/obsidian-dataview/annotation/metadata-tasks/#field-shorthands).
- `scheduled`: The date a task is scheduled to work on; set by `scheduled...` or [shorthand syntaxopen in new window](https://blacksmithgu.github.io/obsidian-dataview/annotation/metadata-tasks/#field-shorthands).
- `annotated`: 如果任务有任何自定义注解，则为真，否则为假.
- `parent`: 这个任务上面的任务的行号，如果存在的话；如果这是一个根级任务，则为空。.
- `blockId`: 该任务/列表元素的块ID, if one has been defined with the `^blockId` syntax; otherwise null.


## 2	Where 过滤器-查函数

```
LIST WHERE file.mtime >= date(today) - dur(1 day)
```

### 2.1	1.  contains() 包含函数

- 对于对象，检查该对象是否有具有给定名称的键
- 对于列表，检查是否有任何数组元素等于给定值
- 对于字符串，检查给定值是否为子字符串

```
contains(file.name, "咖啡豆") = true
# 检查对象 file.name ，包含“咖啡豆”，就返回结果为 真
```
### 2.2	2. date(any) 日期函数

从提供的字符串、日期或链接对象中分析日期（如果可能），否则返回nul

```
date("2020-04-18") 
# 日期对象代表 2020年4月18日

date([[2021-04-16]])
# 给定页面的日期对象，参考file.day
```
### 2.3	3.  dur(any) 从字符串解析时间

从提供的字符串或持续时间分析**持续时间**，失败时返回null

```
dur(8 minutes)
# 8分钟

dur("8 minutes, 4 seconds")
# 8分4秒

dur(dur(8 minutes))
# dur(8 minutes) = <8 minutes>
```

### 2.4	4. 实战2:where带2个函数查询

```
list
from ""
WHERE file.mtime >= date(today) - dur(7 day)
sort file.mtime desc
```

```
table without id
	file.link as 文件名,
	file.folder as "文件夹",
	file.mtime as 修改时间
from "" and -#obsidian
WHERE file.mtime >= date(today) - dur(7 day)
sort file.mtime desc
limit 20
```

## 3	 limit限制显示数量

当你查询的数据过多的时候，可能不想显示太多的内容。那么我们可以控制显示数据的数量，比如只显示10个。语法简单，不多赘述
