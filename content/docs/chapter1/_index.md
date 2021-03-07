---
# Title, summary, and page position.
linktitle: Chapter1
summary: 学习如何使用学术的文档布局发布在线课程、软件文档和教程
weight: 1
icon: book
icon_pack: fas

# Page metadata.
title: Chapter 1
date: "2021-03-27T00:00:00Z"
type: book  # Do not modify.
---

## Flexibility

文档一切!

该特性可用于发布以下内容:

* **Online courses**
* **Project or software documentation**
* **Tutorials**
* **Notes**

`courses` 文件夹可以重命名。例如，对于软件/项目文档，我们可以将其重命名为“docs”;对于创建在线课程，我们可以将其重命名为“tutorials”。

## 删除课程

**To remove these pages, delete the `courses` folder and see below to delete the associated menu link.**

## Update site menu

After renaming or deleting the `courses` folder, you may wish to update any `[[main]]` menu links to it by editing your menu configuration at `config/_default/menus.toml`.

For example, if you delete this folder, you can remove the following from your menu configuration:

```toml
[[main]]
  name = "Courses"
  url = "courses/"
  weight = 50
```

Or, if you are creating a software documentation site, you can rename the `courses` folder to `docs` and update the associated *Courses* menu configuration to:

```toml
[[main]]
  name = "Docs"
  url = "docs/"
  weight = 50
```

## Update the docs menu

If you use the *docs* layout, note that the name of the menu in the front matter should be in the form `[menu.X]` where `X` is the folder name. Hence, if you rename the `courses/example/` folder, you should also rename the menu definitions in the front matter of files within `courses/example/` from `[menu.example]` to `[menu.<NewFolderName>]`.
