---
name: obsidian-gtd
description: >
  This skill should be used when the user wants to set up or manage a GTD (Getting Things Done) task management system in Obsidian. It covers initial setup (installing the Tasks plugin, creating a GTD Dashboard), daily to-do file generation with automatic tag management (#inbox, #today, #later), carrying over incomplete tasks from the previous day, and ongoing daily task review workflows. Trigger when the user mentions "GTD", "to do", "每日任务", "任务管理", "daily todo", "task dashboard", or asks to organize daily tasks in Obsidian.
---

# Obsidian GTD — 每日任务管理

将 Obsidian 笔记库变成轻量级 GTD 任务管理系统。基于 Tasks 插件 + 每日 to do 文件 + tag 分类。

---

## 一、首次设置

首次使用时按顺序完成以下四步。如果用户已完成某步则跳过。

### 1. 安装 Tasks 插件

引导用户操作：

1. 打开 Obsidian → 设置 ⚙️ → 第三方插件
2. 如果"安全模式"开着，点"关闭安全模式"
3. 点"浏览" → 搜索 **Tasks**（作者 Martin Schenck & Clare Macrae）
4. 点"安装" → 点"启用"

确认插件启用后再继续。

### 2. 确定 To Do 文件夹位置

在创建任何文件之前，先询问用户 to do 文件放在哪里：

- 问用户："你的 Obsidian 库里想把每日 to do 放在哪个文件夹？告诉我路径就行，不指定的话默认放在 `每日to do/` 下。"
- 如果用户指定了路径 → 确认路径存在后使用
- 如果用户没指定或说"默认就好" → 使用 `每日to do/` 作为默认文件夹，不存在则创建
- 将确定的路径记住，后续所有文件都放在这个文件夹下

### 3. 创建 GTD Dashboard

在上一步确定的文件夹下创建 `GTD Dashboard.md`。

Dashboard 包含以下区域，每个区域用 Obsidian Tasks 插件的查询语法（即 tasks 代码块）自动聚合任务：

**📥 Inbox — 待整理**（查询条件：`tags include #inbox` + `not done`）

**🎯 Today — 今天要做**（查询条件：`tags include #today` + `not done`）

**📅 Later — 以后再说**（查询条件：`tags include #later` + `not done`）

**✅ 今日已完成**（查询条件：`done on today`）

**✅ 近 7 天已完成**（查询条件：`done after 7 days ago` + `short mode`）

每个查询区域的写法：

```
​```tasks
tags include #inbox
not done
​```
```

将上面的 `#inbox` 替换为对应 tag 即可。

### 4. 设置每日自动化

询问用户希望如何管理每日 to do 的生成：

- 问用户："你希望每天自动帮你整理 to do 吗？可以设定一个时间（比如每天早上 8:30），到点我会自动把昨天没做完的搬过来并问你今天要加什么。也可以选择手动，每次你跟我说的时候我再帮你整理。"
- **如果用户选择自动化** → 再问想几点运行（默认建议 08:30），然后设置定时任务
- **如果用户选择手动** → 不创建自动化，告诉用户每天想整理时跟我说一声就行

无论自动还是手动，每次整理的流程都一样：

1. 读取用户指定文件夹中昨天的文件（YYYY-MM-DD.md），找出所有 `- [ ]` 开头的未完成任务
2. 创建今天的文件，将未完成任务放在"昨日未完成"区域，tag 统一改为 `#today`
3. 问用户今天还要加什么新任务，新任务默认打 `#inbox`

---

## 二、Tag 规则

| Tag | 含义 | 用法 |
|-----|------|------|
| `#inbox` | 📥 收集箱 | 新任务默认放这里 |
| `#today` | 🎯 今天做 | 明确要今天完成的 |
| `#later` | 📅 以后做 | 重要但不急 |

**打 tag 规则：**
- 昨日未完成搬过来 → `#today`
- 用户新增没说优先级 → `#inbox`
- 用户说今天要做 → `#today`
- 用户说不急/以后 → `#later`
- tag 放在任务文字最后面，如：`- [ ] 写文档 #today`

---

## 三、每日 To Do 文件格式

文件名：`YYYY-MM-DD.md`，放在用户指定的文件夹下。

```markdown
# 📋 每日 To Do — YYYY-MM-DD（周X）

## 昨日未完成
- [ ] xxx #today
- [ ] xxx #today

## 今日任务
- [ ] xxx #inbox

## 备注

---
*加油，今天也是充实的一天！💪*
```

如果没有昨日未完成的任务，省略"昨日未完成"区域。

---

## 四、日常操作

### 添加任务
用户说要加任务时，读取当天文件，在"今日任务"下追加，按 tag 规则打标。

### 整理 To Do（自动或手动）
读取昨天文件 → 收集未完成项 → 生成今天文件 → 告诉用户搬了什么过来 → 问还要加什么。

### 改优先级
把任务行末尾的 tag 换掉就行，如 `#inbox` → `#today`。

### 完成任务
用户在 Obsidian 里勾选（`[ ]` 变 `[x]`），Dashboard 自动更新。
