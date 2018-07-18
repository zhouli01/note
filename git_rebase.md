# Git rebase

合并多个commit提交信息，也可以利用rebase进行代码回退，所以一定要记得先合并了commit之后再pull dev的代码

**如果是单人开发:**

在个人分支上先直接用`git commit -m “本次提交信息”`

流程如下：


```
 yaha@Yaha~ /data/code/zuoyebang/assistantdesk  yangzuhao ●  git add models/service/page/assistant/UpdateAssistantInfo.php
 yaha@Yaha~ /data/code/zuoyebang/assistantdesk  yangzuhao ✚  git commit -m"修改班主任添加方法"
 
 yaha@Yaha~ /data/code/zuoyebang/assistantdesk  yangzuhao ●  git add models/service/page/assistant/UpdateAssistantInfo.php
 yaha@Yaha~ /data/code/zuoyebang/assistantdesk  yangzuhao ✚  git commit -m"修改班主任添加方法"
 
 yaha@Yaha~ /data/code/zuoyebang/assistantdesk  yangzuhao  git log
 
 
 
 commit 6e7972876826f3203671d284777091cba27a6423 (HEAD -> yangzuhao)
Author: yangzuhao <yangzuhao@zuoyebang.com>
Date:   Sun Jul 8 23:56:11 2018 +0800

    修改班主任添加方法

commit 8e3f8736e9d07e5ff55d0b398e881f1645e9a54b
Author: yangzuhao <yangzuhao@zuoyebang.com>
Date:   Sun Jul 8 23:48:43 2018 +0800

    修改班主任添加方法

commit bb920d6f0f87853a1aba3e1b174d17161a1c840b
Merge: e41875d49 97d442eb3
Author: likai <likai@zuoyebang.com>
Date:   Sat Jul 7 11:10:33 2018 +0800

    <feat>(loation) : <新增绩效查询人员管理Api接口>

         新增绩效查询人员管理Api接口

```

想要合并刚刚提交的commit信息，则需要运行`git rebase -i bb920d6f0f87853a1aba3e1b174d17161a1c840b`

运行之后则会弹出vim编辑模式，会展示出需要合并的信息，其中

- p, pick 使用该commit
- r, reword 使用该commit, 但是需要修改commit信息
- e, edit 使用该commit，但是停止合并
- s, squash 使用commit，但是把它与前一提交合并
- d, drop 移除该commit

原

**注意：git log和下面的commit id顺序相反，git log中的commit id是以时间最近的在前，rebase是合并的commit id在前，所以在合并commit id应该以时间最前的为pick，之后的都合并在这个上面**

``` 
pick 8e3f8736e 修改班主任添加方法
pick 6e7972876 修改班主任添加方法

```
修改为

```
pick 8e3f8736e 修改班主任添加方法
s 6e7972876 修改班主任添加方法
```

原

```
# This is a combination of 2 commits.
# This is the 1st commit message:

将下面的原有commit信息删除并使用git模板的提交信息

修改班主任添加方法

# This is the commit message #2:

修改班主任添加方法

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Sun Jul 8 23:48:43 2018 +0800
#
# interactive rebase in progress; onto bb920d6f0
# Last commands done (2 commands done):
#    pick 8e3f8736e 修改班主任添加方法
#    s 6e7972876 修改班主任添加方法
# No commands remaining.
# You are currently rebasing branch 'yangzuhao' on 'bb920d6f0'.
#
# Changes to be committed:
#       modified:   models/service/page/assistant/UpdateAssistantInfo.php

```

修改为

```
# This is a combination of 2 commits.
# This is the 1st commit message:

将下面的原有commit信息删除并使用git模板的提交信息

<fix>(loation):修改班主任添加方法

		修改班主任添加方法


# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Sun Jul 8 23:48:43 2018 +0800
#
# interactive rebase in progress; onto bb920d6f0
# Last commands done (2 commands done):
#    pick 8e3f8736e 修改班主任添加方法
#    s 6e7972876 修改班主任添加方法
# No commands remaining.
# You are currently rebasing branch 'yangzuhao' on 'bb920d6f0'.
#
# Changes to be committed:
#       modified:   models/service/page/assistant/UpdateAssistantInfo.php


```

合并后`git log`信息如下

```
commit e63e9e1bfa0485de73d71ea52216ef715226740a (HEAD -> yangzuhao)
Author: yangzuhao <yangzuhao@zuoyebang.com>
Date:   Sun Jul 8 23:48:43 2018 +0800

    <fix>(loation):修改班主任添加方法

		修改班主任添加方法

commit bb920d6f0f87853a1aba3e1b174d17161a1c840b
Merge: e41875d49 97d442eb3
Author: likai <likai@zuoyebang.com>
Date:   Sat Jul 7 11:10:33 2018 +0800

    <feat>(loation) : <新增绩效查询人员管理Api接口>

         新增绩效查询人员管理Api接口

commit 97d442eb352b6a66f1586b1f29f3fa4ccfab4aff
Merge: efbe30ff7 e41875d49
Author: likai <likai@zuoyebang.com>
Date:   Sat Jul 7 11:08:15 2018 +0800

    Merge branch 'dev' into likai
```
