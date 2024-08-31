---
tags:
  - done
progress: 
cssclasses: 
source: https://publish.obsidian.md/chinesehelp/01+2021%E6%96%B0%E6%95%99%E7%A8%8B/Dataview%E7%BF%BB%E8%AF%91+by+%E5%AF%A1%E4%BA%BA
anki: true
---

dataview 呈现:: table task list calendar
<!--ID: 1721204088724-->


[[dataview 呈现]]

dataview 查询:: from "Books" OR #status/wip
<!--ID: 1721204088726-->


[[dataview 查询]]

dataview 过滤:: LIST WHERE file.mtime >= date(today) - dur(1 day)
<!--ID: 1721204088728-->


[[dataview 过滤]]

data 排序:: sort file.mtime desc
<!--ID: 1721204088730-->


dataview 分组:: GROUP BY file.folder
<!--ID: 1721204088732-->

[[dataview排序和分组]]


```dataview
table
file.folder as "文件夹",
file.mtime as 修改时间,
progress/100 as 进度
from #doing 
sort progress
```



- 🍅 (pomodoro::WORK) (duration:: 25m) (begin:: 2024-07-17 15:44) - (end:: 2024-07-17 16:09)

