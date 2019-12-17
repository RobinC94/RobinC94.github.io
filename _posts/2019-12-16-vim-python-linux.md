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

Vim版本为 8.0 以上，支持 python2 和 python3

## 一、配置插件管理器 Vundle
* 创建目录，clone 源代码

```bash
mkdir $HOME/.vim
mkdir $HOME/.vim/bundle
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

* 修改配置文件

这里选择将插件的导入和设置分在两个配置文件当中，导入插件的 plugin 记录在 `~/.vimrc.bundles` 中，插件的具体配置在 `~/.vimrc`中，文末给出这两个文件的全部内容

插件的导入方式是 "Plugin" + 插件名，必须在 `call vundle#begin()` 和 `call vundle#end()` 语句之间

## 二、下载并配置插件

修改好文件后，打开 vim 输入 `:PluginInstall` 会安装配置文件中添加的插件

也可通过命令行直接安装

## 三、插件介绍

### 1.插件管理 Vundle

用于插件管理

* 插件配置

`.vimrc`:
```
set nocompatible              " 去除VI一致性,必须
filetype off		      " 必须
set rtp+=~/.vim/bundle/Vundle.vim/
 
if filereadable(expand("~/.vimrc.bundles"))
  source ~/.vimrc.bundles
endif

" 显示中文帮助
if version >= 603
	set helplang=cn
	set encoding=utf-8
endif
```
`.vimrc.bundles`:
```
if &compatible
  set nocompatible
end
 
filetype off
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" Let Vundle manage Vundle
Plugin 'VundleVim/Vundle.vim'

" add personal plugins here
"{

"}
 
call vundle#end() 
" 侦测文件类型           
filetype on  
" 载入文件类型插件
filetype plugin on
" 为特定文件类型载入相应缩进文件
filetype indent on
```

* 常用命令

| :PluginList                       | 列出所有已配置的插件 |
| --- | ---
| :PluginInstall                    | 安装 `.vimrc.bundle` 中的所有插件
| :PluginInstall!<br>:PlunginUpdate | 更新 `.vimrc.bundle` 中的所有插件
| :PluginInstall <plugin-name>      | 安装某一特定插件
| :PluginClean                      | 清除未使用的插件，需要确认；追加`!`自动批准移除
| :PluginSearch <text-list>         | 搜索过程中，可以在交互式分屏上安装、清理、研究或重新装入同一列表。想自动装入插件，需将插件添加到 `.vimrc.bundles` 文件
| :PluginClean                      | 清除未使用的插件，需要确认；追加`!`自动批准移除
| :PluginSearch foo                 |搜索 foo ; 追加 `!` 清除本地缓存，搜索完成后，可以按下'i'进行安装


### 2.主题 solarized

* 插件配置
```
let g:solarized_termtrans=1
let g:solarized_contrast="normal"
let g:solarized_visibility="normal"
```

### 3.目录树 NERDTree

* 插件配置
```
" F2 open nerdtree, show in left
map <F2> :NERDTreeToggle<CR>
let NERDTreeHighlightCursorline=1
" ignore following file
let NERDTreeIgnore=[ '\.pyc$', '\.pyo$', '\.obj$', '\.o$', '\.so$', '\.egg$', '^\.git$', '^\.svn$', '^\.hg$', '__pycache__' ]
let g:netrw_home='~/bak'
" close vim if the only window left open is a NERDTree
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTreeType") && b:NERDTreeType == "primary") | q | end
" open nerdtree if no file set
autocmd vimenter * if !argc()|NERDTree|endif
let g:nerdtree_tabs_open_on_console_startup=1
```

* 显示 git
需要添加 `Xuyuanp/nerdtree-git-plugin`
```
" git status
let g:NERDTreeIndicatorMapCustom = {
    \ "Modified"  : "✹",
    \ "Staged"    : "✚",
    \ "Untracked" : "✭",
    \ "Renamed"   : "➜",
    \ "Unmerged"  : "═",
    \ "Deleted"   : "✖",
    \ "Dirty"     : "✗",
    \ "Clean"     : "✔︎",
    \ 'Ignored'   : '☒',
    \ "Unknown"   : "?"
    \ }
```
* 常用命令

| Ctrl - w - w   |	光标在 nerdtree 和 vim 编辑窗口 之间切换 |
| --- | --- 
|<leader>nt	     |	打开 nerdtree
| q	             |	关闭 nerdtree
| o	             |	打开选中的文件； 折叠/展开选中的目录
| i	             |	打开选中的文件，与已打开文件纵向排布窗口，并跳转至该窗口
| gi             |	打开选中的文件，与已打开文件纵向排布窗口，但不跳转至该窗口
| s	             |	打开选中的文件，与已打开文件横向排布窗口，并跳转至该窗口
| gs	         |	打开选中的文件，与已打开文件横向排布窗口，但不跳转至该窗口
| t	             |	在新 Tab 中打开选中文件/书签，并跳到新 Tab
| T	             |	在新 Tab 中打开选中文件/书签，但不跳到新 Tab
| x	             |	折叠选中结点的父目录
| X	             |	递归折叠选中结点下的所有目录
| k / j	         |	光标在 Neadtree 上下移动
| <leader>tc	:tabc   | 关闭当前的 tab
| <leader>to	:tabo   | 关闭所有其他 tab
| <leader>ts	:tabs   | 查看所有打开的 tab
| <leader>tp	:tabp   | 前一个 tab
| <leader>tn	:tabn   | 后一个 tab
| ?	             |	显示菜单

### 4. 标签导航

* 插件配置
```
nmap <F3> :TagbarToggle<CR>
let g:tagbar_ctags_bin='/usr/bin/ctags'
let g:tagbar_width=30
autocmd BufReadPost *.cpp,*.c,*.h,*.hpp,*.cc,*.cxx call tagbar#autoopen()
let g:jedi#auto_initialization = 1
```

* 常用命令
| Ctrl - w - w		| 光标在 Tagbar 和 vim 编辑窗口 之间切换 |
| --- | --- 
| <leader>tb		| 打开 tagbar
| q					| 关闭 tagbar
| j, k				| 上下移动光标
| o（+/-）			| 折叠 / 展开标签集合
| p					| 跳转到选中的标签，但光标仍停留在 Tagbar
| P					| 打开预览窗口显示标签内容，光标仍停留在 Tagbar，回车 光标跳转至 vim 编辑窗口标签所在位置，关闭预览窗口
| *					| 展开所有标签
| =					| 折叠所有标签
| x					| 展开 / 缩小标

### 5.文件搜索

### 6.状态栏优化

### 7.自动补全

### 8.运行 python
