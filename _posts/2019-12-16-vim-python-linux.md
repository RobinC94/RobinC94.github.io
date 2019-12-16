---
layout: post
title:  "Vim for Python"
date:   2019-12-16
excerpt: "Deploy python develop & debug env with Vim on Linux."
tag:
- python
- coding
- vim
---

Vim版本为 8.0 以上

## 一、配置插件管理器 Vundle
* 创建目录，clone 源代码

```
mkdir $HOME/.vim
mkdir $HOME/.vim/bundle
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

* 修改配置文件

这里选择将插件的导入和设置分在两个配置文件当中，导入插件的 plugin 记录在 ~/.vimrc.bundles 中，插件的具体配置在 ~/.vimrc 中，文末给出这两个文件的全部内容

插件的导入方式是 "Plugin" + 插件名，必须在 "call vundle#begin()和call vundle#end() 语句之间

## 二、下载并配置插件

修改好文件后，打开 vim 输入 ":PluginInstall" 会安装配置文件中添加的插件

也可通过命令行直接安装

## 三、插件介绍

### 1.插件管理 Vundle

用于插件管理

* 插件配置

* 常用命令

### 2.主题 solarized

* 插件配置

### 3.目录树 NERDTree

* 插件配置

* 常用命令

### 4. 标签导航

* 插件配置

* 常用命令

### 5.文件搜索

### 6. 状态栏优化