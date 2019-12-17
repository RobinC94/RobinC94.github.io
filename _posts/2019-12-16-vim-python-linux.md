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
| :PluginSearch foo                 |搜索 foo ; 追加 `!` 清除本地缓存，搜索完成后，可以按下`i`进行安装


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
map <leader>nt :NERDTreeToggle<CR>
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
| \nt	     |	打开 nerdtree
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
| \tc	:tabc   | 关闭当前的 tab
| \to	:tabo   | 关闭所有其他 tab
| \ts	:tabs   | 查看所有打开的 tab
| \tp	:tabp   | 前一个 tab
| \tn	:tabn   | 后一个 tab
| ?	             |	显示菜单

### 4. 标签导航

* 插件配置

首先需要安装 ctags

```bash
sudo apt-get install ctags
```

配置文件：

```
nmap <leader>tb :TagbarToggle<CR>
let g:tagbar_ctags_bin='/usr/bin/ctags'
let g:tagbar_width=30
autocmd BufReadPost *.cpp,*.c,*.h,*.hpp,*.cc,*.cxx call tagbar#autoopen()
let g:jedi#auto_initialization = 1
```

* 常用命令

| Ctrl - w - w		| 光标在 Tagbar 和 vim 编辑窗口 之间切换 |
| --- | --- 
| \tb		| 打开 tagbar
| q					| 关闭 tagbar
| j, k				| 上下移动光标
| o（+/-）			| 折叠 / 展开标签集合
| p					| 跳转到选中的标签，但光标仍停留在 Tagbar
| P					| 打开预览窗口显示标签内容，光标仍停留在 Tagbar，回车 光标跳转至 vim 编辑窗口标签所在位置，关闭预览窗口
| *					| 展开所有标签
| =					| 折叠所有标签
| x					| 展开 / 缩小标

### 5.文件搜索

* 插件配置

```
" 打开ctrlp搜索
let g:ctrlp_map = '<leader>ff'
let g:ctrlp_cmd = 'CtrlP'
" 相当于mru功能，show recently opened files
map <leader>fp :CtrlPMRU<CR>
" set wildignore+=*/tmp/*,*.so,*.swp,*.zip     " MacOSX/Linux"
let g:ctrlp_custom_ignore = {
    \ 'dir':  '\v[\/]\.(git|hg|svn|rvm)$',
    \ 'file': '\v\.(exe|so|dll|zip|tar|tar.gz)$',
    \ }
"\ 'link': 'SOME_BAD_SYMBOLIC_LINKS',
let g:ctrlp_working_path_mode=0
let g:ctrlp_match_window_bottom=1
let g:ctrlp_max_height=15
let g:ctrlp_match_window_reversed=0
let g:ctrlp_mruf_max=500
let g:ctrlp_follow_symlinks=1
```

* 常用命令


### 6.状态栏优化

* 插件配置

```
let g:airline_powerline_fonts                   = 1 " 使用 powerline 打过补丁的字体
let g:airline#extensions#tabline#enabled        = 1 " 开启 tabline
let g:airline#extensions#tabline#buffer_nr_show = 1 " 显示 buffer 编号
let g:airline#extensions#ale#enabled            = 1 " enable ale integration
 
if !exists('g:airline_symbols')
    let g:airline_symbols = {}
endif
let g:airline_left_sep          = '⮀'
let g:airline_left_alt_sep      = '⮁'
let g:airline_right_sep         = '⮂'
let g:airline_right_alt_sep     = '⮃'
let g:airline_symbols.crypt     = '?'
let g:airline_symbols.linenr    = '⭡'
let g:airline_symbols.branch    = '⭠'
" 切换 buffer
nnoremap [b :bp<CR>
nnoremap ]b :bn<CR>
" 关闭当前 buffer
noremap <C-x> :w<CR>:bd<CR>
" <leader>1~9 切到 buffer1~9
map <leader>1 :b 1<CR>
map <leader>2 :b 2<CR>
map <leader>3 :b 3<CR>
map <leader>4 :b 4<CR>
map <leader>5 :b 5<CR>
map <leader>6 :b 6<CR>
map <leader>7 :b 7<CR>
map <leader>8 :b 8<CR>
map <leader>9 :b 9<CR>
```

* 常用命令

| \]b / \[b   | 切换buffer |
| --- | --- 
| \1~\9     | 切换至对应 num 的buffer
| Ctrl + X  | 关闭当前 buffer |

### 7.自动补全

* 插件配置

首先需要安装依赖库

```bash
sudo apt install build-essential cmake python3-dev
```

在 `:PluginInstall` 之后只是完成了下载，需要手动安装
进入 `~/.vim/bundle/YouCompleteMe` 目录，运行

```bash
python3 install.py --clang-completer
```

配置：

```
let g:ycm_global_ycm_extra_conf = '~/.vim/bundle/YouCompleteMe/third_party/ycmd/.ycm_extra_conf.py'    
let g:ycm_min_num_of_chars_for_completion               = 2 " 输入第 2 个字符开始补全                  
let g:ycm_seed_identifiers_with_syntax                  = 1 " 语法关键字补全    
let g:ycm_complete_in_comments                          = 1 " 在注释中也可以补全    
let g:ycm_complete_in_strings                           = 1 " 在字符串输入中也能补全    
let g:ycm_collect_identifiers_from_tag_files            = 1 " 使用 ctags 生成的 tags 文件    
let g:ycm_collect_identifiers_from_comments_and_strings = 1 " 注释和字符串中的文字也会被收入补全    
let g:ycm_cache_omnifunc                                = 0 " 每次重新生成匹配项，禁止缓存匹配项    
let g:ycm_use_ultisnips_completer                       = 0 " 不查询 ultisnips 提供的代码模板补全，如果需要，设置成 1 即可
let g:ycm_show_diagnostics_ui                           = 0 " 禁用语法检查
let g:ycm_key_list_select_completion   = ['<Down>']   " 选择下一条补全，Default: ['<TAB>', '<Down>']
let g:ycm_key_list_previous_completion = ['<Up>']     " 选择上一条补全，Default: ['<S-TAB>', '<Up>']
let g:ycm_key_list_stop_completion     = ['<Enter>']  " 中止此次补全，Default: ['<C-y>'] 
```

* 常用命令

| Down	| 选择下一条补全 |
| --- | ---
| Up		| 选择上一条补全
| Enter	| 中止此次补全 / 选中补全

### 8.运行 python

* 配置

```
map <F5> :call CompileRunGcc()<CR>
    func! CompileRunGcc()
        exec "w"
if &filetype == 'c'
            exec "!g++ % -o %<"
            exec "!time ./%<"
elseif &filetype == 'cpp'
            exec "!g++ % -o %<"
            exec "!time ./%<"
elseif &filetype == 'java'
            exec "!javac %"
            exec "!time java %<"
elseif &filetype == 'sh'
            :!time bash %
elseif &filetype == 'python'
            exec "!time python %"
elseif &filetype == 'html'
            exec "!firefox % &"
elseif &filetype == 'go'
    "        exec "!go build %<"
            exec "!time go run %"
elseif &filetype == 'mkd'
            exec "!~/.vim/markdown.pl % > %.html &"
            exec "!firefox %.html &"
endif
    endfunc
```

## 四、配置文件记录

* `.vimrc.bundles`:

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
" 目录树导航
Plugin 'scrooloose/nerdtree'
Plugin 'Xuyuanp/nerdtree-git-plugin'
" 主题风格
Plugin 'altercation/vim-colors-solarized'
" 标签导航
Plugin 'majutsushi/tagbar'
" 文件搜索
Plugin 'kien/ctrlp.vim'
" 状态栏优化
Plugin 'vim-airline/vim-airline'
" 自动补全
Plugin 'Valloric/YouCompleteMe'
" 括号匹配
Plugin 'jiangmiao/auto-pairs'

"}
 
call vundle#end() 
" 侦测文件类型           
filetype on  
" 载入文件类型插件
filetype plugin on
" 为特定文件类型载入相应缩进文件
filetype indent on
```

* `.vimrc`:

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

""""""""""""""""""""""""""""""
" tagbar标签导航
""""""""""""""""""""""""""""""
nmap <leader>tb :TagbarToggle<CR>
let g:tagbar_ctags_bin='/usr/bin/ctags'
let g:tagbar_width=30
autocmd BufReadPost *.cpp,*.c,*.h,*.hpp,*.cc,*.cxx call tagbar#autoopen()
let g:jedi#auto_initialization = 1

""""""""""""""""""""""""""""""
" 主题 solarized
""""""""""""""""""""""""""""""
let g:solarized_termtrans=1
let g:solarized_contrast="normal"
let g:solarized_visibility="normal"

""""""""""""""""""""""""""""""
" 配色方案
""""""""""""""""""""""""""""""
set background=dark
set t_Co=256
colorscheme solarized

""""""""""""""""""""""""""""""
" 目录文件导航NERD-Tree
""""""""""""""""""""""""""""""
" \nt open nerdtree, show in left
map <leader>nt :NERDTreeToggle<CR>
let NERDTreeHighlightCursorline=1
" ignore following file
let NERDTreeIgnore=[ '\.pyc$', '\.pyo$', '\.obj$', '\.o$', '\.so$', '\.egg$', '^\.git$', '^\.svn$', '^\.hg$', '__pycache__' ]
let g:netrw_home='~/bak'
" close vim if the only window left open is a NERDTree
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTreeType") && b:NERDTreeType == "primary") | q | end
" open nerdtree if no file set
autocmd vimenter * if !argc()|NERDTree|endif
let g:nerdtree_tabs_open_on_console_startup=1
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

""""""""""""""""""""""""""""""
" ctrlp文件搜索
""""""""""""""""""""""""""""""
" 打开ctrlp搜索
let g:ctrlp_map = '<leader>ff'
let g:ctrlp_cmd = 'CtrlP'
" 相当于mru功能，show recently opened files
map <leader>fp :CtrlPMRU<CR>
" set wildignore+=*/tmp/*,*.so,*.swp,*.zip     " MacOSX/Linux"
let g:ctrlp_custom_ignore = {
    \ 'dir':  '\v[\/]\.(git|hg|svn|rvm)$',
    \ 'file': '\v\.(exe|so|dll|zip|tar|tar.gz)$',
    \ }
"\ 'link': 'SOME_BAD_SYMBOLIC_LINKS',
let g:ctrlp_working_path_mode=0
let g:ctrlp_match_window_bottom=1
let g:ctrlp_max_height=15
let g:ctrlp_match_window_reversed=0
let g:ctrlp_mruf_max=500
let g:ctrlp_follow_symlinks=1

""""""""""""""""""""""""""""""
" 状态栏优化
""""""""""""""""""""""""""""""
let g:airline_powerline_fonts                   = 1 " 使用 powerline 打过补丁的字体
let g:airline#extensions#tabline#enabled        = 1 " 开启 tabline
let g:airline#extensions#tabline#buffer_nr_show = 1 " 显示 buffer 编号
let g:airline#extensions#ale#enabled            = 1 " enable ale integration
 
""""""""""""""""""""""""""""""
" 状态栏显示图标设置
""""""""""""""""""""""""""""""
if !exists('g:airline_symbols')
    let g:airline_symbols = {}
endif
let g:airline_left_sep          = '⮀'
let g:airline_left_alt_sep      = '⮁'
let g:airline_right_sep         = '⮂'
let g:airline_right_alt_sep     = '⮃'
let g:airline_symbols.crypt     = '?'
let g:airline_symbols.linenr    = '⭡'
let g:airline_symbols.branch    = '⭠'
" 切换 buffer
nnoremap [b :bp<CR>
nnoremap ]b :bn<CR>
" 关闭当前 buffer
noremap <C-x> :w<CR>:bd<CR>
" <leader>1~9 切到 buffer1~9
map <leader>1 :b 1<CR>
map <leader>2 :b 2<CR>
map <leader>3 :b 3<CR>
map <leader>4 :b 4<CR>
map <leader>5 :b 5<CR>
map <leader>6 :b 6<CR>
map <leader>7 :b 7<CR>
map <leader>8 :b 8<CR>
map <leader>9 :b 9<CR>

""""""""""""""""""""""""""""""
" 自动补全
""""""""""""""""""""""""""""""
let g:ycm_global_ycm_extra_conf = '~/.vim/bundle/YouCompleteMe/third_party/ycmd/.ycm_extra_conf.py'    
let g:ycm_min_num_of_chars_for_completion               = 2 " 输入第 2 个字符开始补全                  
let g:ycm_seed_identifiers_with_syntax                  = 1 " 语法关键字补全    
let g:ycm_complete_in_comments                          = 1 " 在注释中也可以补全    
let g:ycm_complete_in_strings                           = 1 " 在字符串输入中也能补全    
let g:ycm_collect_identifiers_from_tag_files            = 1 " 使用 ctags 生成的 tags 文件    
let g:ycm_collect_identifiers_from_comments_and_strings = 1 " 注释和字符串中的文字也会被收入补全    
let g:ycm_cache_omnifunc                                = 0 " 每次重新生成匹配项，禁止缓存匹配项    
let g:ycm_use_ultisnips_completer                       = 0 " 不查询 ultisnips 提供的代码模板补全，如果需要，设置成 1 即可
let g:ycm_show_diagnostics_ui                           = 0 " 禁用语法检查
let g:ycm_key_list_select_completion   = ['<Down>']   " 选择下一条补全，Default: ['<TAB>', '<Down>']
let g:ycm_key_list_previous_completion = ['<Up>']     " 选择上一条补全，Default: ['<S-TAB>', '<Up>']
let g:ycm_key_list_stop_completion     = ['<Enter>']  " 中止此次补全，Default: ['<C-y>'] 

""""""""""""""""""""""""""""""
" 一键运行代码
""""""""""""""""""""""""""""""
map <F5> :call CompileRunGcc()<CR>
    func! CompileRunGcc()
        exec "w"
if &filetype == 'c'
            exec "!g++ % -o %<"
            exec "!time ./%<"
elseif &filetype == 'cpp'
            exec "!g++ % -o %<"
            exec "!time ./%<"
elseif &filetype == 'java'
            exec "!javac %"
            exec "!time java %<"
elseif &filetype == 'sh'
            :!time bash %
elseif &filetype == 'python'
            exec "!time python %"
elseif &filetype == 'html'
            exec "!firefox % &"
elseif &filetype == 'go'
    "        exec "!go build %<"
            exec "!time go run %"
elseif &filetype == 'mkd'
            exec "!~/.vim/markdown.pl % > %.html &"
            exec "!firefox %.html &"
endif
    endfunc

""""""""""""""""""""""""""""""
" 添加个人文件头
""""""""""""""""""""""""""""""
"新建.c,.h,.sh,.java,.py文件，自动插入文件头 
autocmd BufNewFile *.cpp,*.[ch],*.sh,*.java,*.py exec ":call SetTitle()" 
 
""定义函数SetTitle，自动插入文件头 
func SetTitle() 
	if &filetype == 'sh' || &filetype == 'python'
		call setline(1, "##################################################") 
		call append(line("."), "# File Name: ".expand("%")) 
		call append(line(".")+1, "# Author: Robin Chen") 
		call append(line(".")+2, "# mail: chenruobing@sensetime.com") 
		call append(line(".")+3, "# Created Time: ".strftime("%Y-%m-%d")) 
		call append(line(".")+4, "##################################################") 
		if &filetype == 'sh'
			call append(line(".")+5, "#!/bin/zsh")
		else
			call append(line(".")+5, "#!/usr/bin/python")
		endif
		call append(line(".")+8, "")

	else
		call setline(1, "/*************************************************************************") 
		call append(line("."), " > File Name: ".expand("%")) 
		call append(line(".")+1, " > Author: Robin Chen") 
		call append(line(".")+2, " > Mail: chenruobing@sensetime.com") 
		call append(line(".")+3, " > Created Time: ".strftime("%Y-%m-%d")) 
		call append(line(".")+4, " ************************************************************************/") 
		call append(line(".")+5, "")
	endif
	"新建文件后，自动定位到文件末尾
	autocmd BufNewFile * normal G
endfunc
```
