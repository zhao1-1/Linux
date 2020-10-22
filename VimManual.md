

## 10.7 vim

### （1）zhao1-1/.vim/vimrc

（1-1）配置文件及插件：`$ vim ~/.vimrc`

```shell
" Specify a directory for plugins
" - For Neovim: stdpath('data') . '/plugged'
" - Avoid using standard Vim directory names like 'plugin'
call plug#begin('~/.vim/plugged')

" 在此位置添加插件名称

call plug#end()
```



（1-2）查询所有配置选项

```
进入vim

“ 查询所有的配置选项
:h option-list
```

（1-3）映射

1. 模式映射：
   + map
   + nmap
   + vmap
   + imap
   + noremap
   + nnoremap
   + vnoremap
   + inoremap
2. 递归与非递归（全用非递归！）：
   + 递归映射：map
   + 非递归映射：noremap
   + 注意：任何时候都要使用非递归映射！！



（1-4）插件配置：

（1-5）vim开源配置：

1. `SpaceVim/SpaceVim`
2. 

（1-6）我自己的配置文件：（放在Git仓库里）

```shell
"    ______                  ____  _
"   |__  / |__   __ _  ___  | __ )(_)_ __
"     / /| '_ \ / _` |/ _ \ |  _ \| | '_ \
"    / /_| | | | (_| | (_)  | |_) | | | | |
"   /____|_| |_|\__,_|\___/ |____/|_|_| |_|

"   __     __  ___   __  __   ____     ____
"   \ \   / / |_ _| |  \/  | |  _ \   / ___|
"    \ \ / /   | |  | |\/| | | |_) | | |
"     \ V /    | |  | |  | | |  _ <  | |___
"      \_/    |___| |_|  |_| |_| \_\  \____|

" Author: https://github.com/zhao1-1
" ------------------------------------------
" figlet ZhaoBin-VIMRC
" Call figlet
noremap tx :r !figlet 
" -------------------------------------------

" ------------------
" ===Editor Setup===
" ------------------
" echom "Welcome to my vimrc ~(>^.^<)~"

" ===
" === Main code Display
" ===
" 设置<LEADER>为空格键
let mapleader="\<space>"
" 代码高亮--开
syntax on
" 打开行号
set number
" 以当前行号为起点，上下计算行号
"set norelativenumber
" 当前光标位置显示一条线
set cursorline
" 字不会超出当前窗口，会自动换行
set wrap
" 输入任何字符，在vim右下角会提示显示该字符
set showcmd
" 在vim：进入指令模式下，会tab帮你自动补全
set wildmenu



" ===
" === Systm
" ===
" 让vim配置保存后立刻生效
" autocmd BufWritePost $MYVIMRC source $MYVIMRC
" 大部分插件需要的配置
set nocompatible
" 自动检测文件类型
filetype on
filetype indent on
filetype plugin on
filetype plugin indent on
" 让vim可以使用鼠标
set mouse=a
" 指定编码
set encoding=utf-8


" 终端跑vim配色不对，加此项可修复
let &t_ut=''
" tab键代表缩进长度为2
" set noexpandtab
set expandtab
set tabstop=2
set shiftwidth=2
set softtabstop=2
" 显示行尾空格
set list
set listchars=tab:\|\ ,trail:▫
" 滚屏的时候上下始终留5行
set scrolloff=5
" 设置缩进
set tw=0
set indentexpr=
" 回退到行首的时候再回退至行尾
set backspace=indent,eol,start
" 代码折叠方式
set foldmethod=indent
set foldlevel=99
" json 格式化
"com! FormatJSON %!python3 -m json.tool
" Cursor shape（vim三种不同模式下光标不同样式）
let &t_SI = "\<Esc>]50;CursorShape=1\x7"
"let &t_SR = "\<Esc>]50;CursorShape=2\x7"
let &t_EI = "\<Esc>]50;CursorShape=0\x7"
" 设置下边状态栏为2
set laststatus=2
" 在当前文件目录下执行命令
set autochdir
" 打开vim，光标会停留在上次关闭时候的位置
"au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif



" ===
" === 全局键位替换
" ===
noremap s <nop>
" noremap j k
" noremap k j
" 多行，多列移动
noremap H 8h
noremap L 8l
noremap J 10j
noremap K 10k
" $ key: go to the start of the line
noremap $ 0
" 0 key: go to the end of the line
noremap 0 $
" 对调; :两个键位
noremap ; :
noremap : ;
" Join lines
noremap <C-j> J
" Help
noremap <C-k> K
" {  } 变成了[]，[代表段首，]代表段尾
"noremap { [
"noremap } ]
"noremap [ {
"noremap ] }
" 保存、退出、生效文件
noremap <LEADER>w :w<CR>
noremap <LEADER>q :q<CR>
noremap <LEADER>r :source $MYVIMRC<CR>
" Open your ~/.vim/vimrc File
nnoremap <LEADER>mv :set splitright<CR>:vsplit $MYVIMRC<CR>
" 在Insert模式下，按两下j键，相当于<Esc>，并且光标回到最初编辑位置
inoremap jj <Esc>`^
" 在Insert模式下，Ctrl+a，移动光标至行首
inoremap <C-a> <Esc>I
" 在Insert模式下，Ctrl+e，移动光标至行尾
inoremap <C-e> <Esc>A
" 在Insert模式下，Ctrl+u，删除或剪切光标之前的所有内容；
inoremap <C-u> <Esc>ld0I
" 在Insert模式下，Ctrl+k，删除或剪切光标之后的所有内容；
inoremap <C-k> <Esc>ld$A
" 在Insert模式下，Ctrl+y，粘贴ctrl+u或ctrl+K剪切的内容
inoremap <C-y> <Esc>pa
" 在visual模式下，取消按u和U转换大小写，而是跟Normal一样，用~键转换大小写
"vnoremap u <nop>
"vnoremap U <nop>
" Press space twice to jump to the next '<++>' and edit it
noremap <LEADER><LEADER> <ESC>/<++><CR>:nohlsearch<CR>c4l
" Find duplicate words
map <LEADER>fd /\(\<\w\+\>\)\_s*\1



" ===
" === Search Set
" ===
" 高亮搜索结果
set hlsearch
" 启动的时候不要高亮, 如果不设置这个，vim会自动记住你上次高亮情况
exec "nohlsearch"
" 在搜索中，边输入边高亮
set incsearch
" 搜索的时候忽略大小写
set ignorecase
" 搜索时候智能大小写
set smartcase
" 搜索时候用“n”/“N”前往下一条/上一条，并且移动至屏幕中央
noremap n nzz
noremap N Nzz
" 取消搜索高亮
noremap <LEADER><CR> :nohlsearch<CR>


" ===
" === Splite Window
" ===
" 向右分屏，并且光标放在右边；
noremap sr :set splitright<CR>:vsplit<CR>
" 向左分屏，并且光标在左边；
noremap sl :set nosplitright<CR>:vsplit<CR>
" 向上分屏，并且光标在上边；
noremap su :set nosplitbelow<CR>:split<CR>
" 向下分屏，并且光标在下边;
noremap sd :set splitbelow<CR>:split<CR>

" 分屏方向切换
noremap sv <C-w>t<C-w>H
noremap sh <C-w>t<C-w>K

" use <LEADER>+h/j/k/l switch window
noremap <LEADER>h <C-w>h
noremap <LEADER>j <C-w>j
"noremap <LEADER>k <C-w>k
noremap <LEADER>l <C-w>l

" 调整分屏各窗口的尺寸大小
noremap <LEADER><up> :res +5<CR>
noremap <LEADER><down> :res -5<CR>
noremap <LEADER><left> :vertical resize-5<CR>
noremap <LEADER><right> :vertical resize+5<CR>


" ===
" === Buffer
" ===
" 切换Buffer
"nnoremap <silent> [b :bprevious<CR>
"nnoremap <silent> [n :bnext<CR>


" ===
" === Tabs
" ===
" 打开新的标签页
noremap ta :tabe<CR>
" 移动到左边的标签页
noremap t- :-tabnext<CR>
" 移动到右边的标签页
noremap t= :+tabnext<CR>



" Spelling Check with <space>sc
noremap <LEADER>sc :set spell!<CR>
noremap <C-x> ea<C-x>s
inoremap <C-x> <Esc>ea<C-x>s





" ===
" === Plug
" ===

call plug#begin('~/.vim/plugged')

" This plugin provides a start screen for Vim and Neovim.
Plug 'mhinz/vim-startify'

" The statusline is the colored line at the bottom
Plug 'vim-airline/vim-airline'
" Plug 'vim-airline/vim-airline-themes'
" Plug 'bling/vim-bufferline'
" Plug 'liuchengxu/space-vim-theme'

" Colorscheme
Plug 'connorholyday/vim-snazzy'
Plug 'w0ng/vim-hybrid'
Plug 'altercation/vim-colors-solarized'
Plug 'morhetz/gruvbox'

" Add indent line especially in Python.
Plug 'Yggdroot/indentLine'

" File manager and navigation
Plug 'scrooloose/nerdtree', { 'on': 'NERDTreeToggle' }
Plug 'Xuyuanp/nerdtree-git-plugin'
"Plug 'ctrlpvim/ctrlp.vim'      "Search and Navigation

" Fuzzy Search by fzf.vim
Plug '/usr/local/opt/fzf'
Plug 'junegunn/fzf.vim'

" find and replace text through multiple files
"Plug 'brooth/far.vim'

" quick Move in current screen page
Plug 'easymotion/vim-easymotion'

" language is all about "surroundings": parentheses, brackets, quotes, XML tags, and more. The plugin provides mappings to easily delete, change and add such surroundings in pairs.
Plug 'tpope/vim-surround'

" Golang
"Plug 'fatih/vim-go', { 'do': ':GoUpdateBinaries' }      代码补全、重构、跳转、自动格式化、自动导入等功能

" Python
Plug 'python-mode/python-mode', { 'for': 'python', 'branch': 'develop' }

" HTML, CSS, JavaScript, PHP, JSON, etc.
Plug 'elzr/vim-json'
Plug 'hail2u/vim-css3-syntax'
Plug 'spf13/PIV', { 'for' :['php', 'vim-plug'] }
Plug 'gko/vim-coloresque', { 'for': ['vim-plug', 'php', 'html', 'javascript', 'css', 'less'] }
Plug 'pangloss/vim-javascript', { 'for' :['javascript', 'vim-plug'] }
Plug 'mattn/emmet-vim'

" Taglist  代码大纲（browse the tags of the current file and get an overview of its structure）
Plug 'majutsushi/tagbar', { 'on': 'TagbarOpenAutoClose' }

" Word highlighting and navigation throught out the buffer.
" * Highlight with <Leader>k
" * Navigate highlighted words with n and N
" * Clear every word highlight with <Leader>K throughout the buffer
Plug 'lfv89/vim-interestingwords'

" Auto Complete
"Plug 'Valloric/YouCompleteMe'

" Code Format  代码格式化
Plug 'sbdchd/neoformat'

" Error checking  静态检查
Plug 'w0rp/ale'

" Comment Code by Useing 'gcc' to comment code and 'gcgc' to cancel all code when you in NormalMode,And Using 'gc' to comment or cancel code when you in VisualMode.
Plug 'tpope/vim-commentary'

" Git
Plug 'rhysd/conflict-marker.vim'
Plug 'tpope/vim-fugitive'
Plug 'mhinz/vim-signify'
Plug 'gisphm/vim-gitignore', { 'for': ['gitignore', 'vim-plug'] }

" Undo Tree
Plug 'mbbill/undotree/'

" Other visual enhancement
"Plug 'itchyny/vim-cursorword'

" Markdown
Plug 'iamcco/markdown-preview.nvim'
", { 'do': { -> mkdp#util#install_sync() }, 'for' :['markdown', 'vim-plug'] }
Plug 'dhruvasagar/vim-table-mode', { 'on': 'TableModeToggle' }
Plug 'vimwiki/vimwiki'

" Bookmarks
Plug 'kshenoy/vim-signature'

" Other useful utilities
Plug 'terryma/vim-multiple-cursors'
Plug 'junegunn/goyo.vim' " distraction free writing mode
Plug 'godlygeek/tabular' " in command mode, type ';Tabularize /=' to align the =
Plug 'gcmt/wildfire.vim' " in Visual mode, type i' to select all text in '', or type i) i] i} ip

" Chinese Input Fuck
Plug 'lyokha/vim-xkbswitch'

" Dependencies
Plug 'MarcWeber/vim-addon-mw-utils'
Plug 'kana/vim-textobj-user'
Plug 'fadein/vim-FIGlet'

call plug#end()


" ===
" === ColorScheme
" ===
"let g:SnazzyTransparent = 1
set background=dark
colorscheme gruvbox

" ===
" === Yggdroot/indentLine
" ===
let g:indentLine_char = '|'
let g:indentLine_color_term = 238
let g:indentLine_color_gui = '#333333'
silent! unmap <LEADER>ig
autocmd WinEnter * silent! unmap <LEADER>ig


" ===
" === NERDTree
" ===
noremap gn :NERDTreeToggle<CR>
let NERDTreeShowHidden=1    "show hidden file
let NERDTreeMapToggleHidden = "zh"
"let NERDTreeMapOpenExpl = ""
let NERDTreeMapUpdir = "N"
"let NERDTreeMapUpdirKeepOpen = "n"
"let NERDTreeMapOpenSplit = ""
"let NERDTreeOpenVSplit = "I"
"let NERDTreeMapActivateNode = "i"
"let NERDTreeMapOpenInTab = "o"
"let NERDTreeMapOpenInTabSilent = "O"
"let NERDTreeMapPreview = ""
"let NERDTreeMapCloseDir = ""
"let NERDTreeMapChangeRoot = "l"
"let NERDTreeMapMenu = ","


" ==
" == NERDTree-git
" ==
let g:NERDTreeIndicatorMapCustom = {
    \ "Modified"  : "✹",
    \ "Staged"    : "✚",
    \ "Untracked" : "✭",
    \ "Renamed"   : "➜",
    \ "Unmerged"  : "═",
    \ "Deleted"   : "✖",
    \ "Dirty"     : "✗",
    \ "Clean"     : "✔︎",
    \ "Unknown"   : "?"
    \ }


" ==
" == ctrlp
" ==
"let g:ctrlp_map = '<C-p>'


" ==
" == easymotion
" ==
nmap ff <Plug>(easymotion-s2)


" ==
" == vim-surround
" ==
" type ysks' to wrap the word with '' or type cs'` to change 'word' to `word`
" ys iw '     （you add a surrounding ' in a word）
" ys ip )     （you add a surrounding ) in paragraph）
" cs ) ]      （change a surrounding from () to []）
" cs } >      （change a surrounding from {} to <>）
" ds '         ( delete a surrounding '')



" ==
" == fzf.vim
" ==
" Ag[Pattern]：模糊搜索字符串
" Files[Path]：模糊搜索目录


" ==
" == pthon-mode
" ==
"let g:pymode_python = 'python3'
"let g:pymode_trim_whitespaces = 1
"let g:pymode_options_max_line_length = 120


"" ===
"" === YouCompleteME
"" ===
"nnoremap gd :YcmCompleter GoToDefinitionElseDeclaration<CR>
"nnoremap g/ :YcmCompleter GetDoc<CR>
"nnoremap gt :YcmCompleter GetType<CR>
"nnoremap gr :YcmCompleter GoToReferences<CR>
"let g:ycm_autoclose_preview_window_after_completion=0
"let g:ycm_autoclose_preview_window_after_insertion=1
"let g:ycm_use_clangd = 0
"let g:ycm_python_interpreter_path = "/bin/python3"
"let g:ycm_python_binary_path = "/bin/python3"


" ===
" === ale
" ===
let b:ale_linters = ['pylint']
let b:ale_fixers = ['autopep8', 'yapf']


"" ===
"" === Taglist
"" ===
noremap <silent> <LEADER>t :TagbarOpenAutoClose<CR>
" nnoremap <LEADER>t :TagbarToggle<CR>


" ===
" === MarkdownPreview
" ===
" let g:mkdp_auto_start = 0
" let g:mkdp_auto_close = 1
" let g:mkdp_refresh_slow = 0
" let g:mkdp_command_for_global = 0
" let g:mkdp_open_to_the_world = 0
" let g:mkdp_open_ip = ''
" let g:mkdp_browser = 'Safari'     " 选择打开的浏览器
" let g:mkdp_echo_preview_url = 0
" let g:mkdp_browserfunc = ''
" let g:mkdp_preview_options = {
"     \ 'mkit': {},
"     \ 'katex': {},
"     \ 'uml': {},
"     \ 'maid': {},
"     \ 'disable_sync_scroll': 0,
"     \ 'sync_scroll_type': 'middle',
"     \ 'hide_yaml_meta': 1
"     \ }
" let g:mkdp_markdown_css = ''
" let g:mkdp_highlight_css = ''
" let g:mkdp_port = ''
" let g:mkdp_page_title = '「${name}」'
" let g:vimwiki_list = [{'path': '~/vimwiki/', 'syntax': 'markdown', 'ext': '.md'}]


" ===
" === vim-table-mode
" ===
noremap <LEADER>T :TableModeToggle<CR>

" ===
" === Python-syntax
" ===
let g:python_highlight_all = 1
" let g:python_slow_sync = 0


" ===
" === Goyo 极简格式写作
" ===
noremap <LEADER>gy :Goyo<CR>


" ===
" === vim-signature 给文件添加书签
" ===
"let g:SignatureMap = {
"         \ 'Leader'             :  "m",
"         \ 'PlaceNextMark'      :  "m,",
"         \ 'ToggleMarkAtLine'   :  "m.",
"         \ 'PurgeMarksAtLine'   :  "dm-",
"         \ 'DeleteMark'         :  "dm",
"         \ 'PurgeMarks'         :  "dm/",
"         \ 'PurgeMarkers'       :  "dm?",
"         \ 'GotoNextLineAlpha'  :  "m<LEADER>",
"         \ 'GotoPrevLineAlpha'  :  "",
"         \ 'GotoNextSpotAlpha'  :  "m<LEADER>",
"         \ 'GotoPrevSpotAlpha'  :  "",
"         \ 'GotoNextLineByPos'  :  "",
"         \ 'GotoPrevLineByPos'  :  "",
"         \ 'GotoNextSpotByPos'  :  "mn",
"         \ 'GotoPrevSpotByPos'  :  "mp",
"         \ 'GotoNextMarker'     :  "",
"         \ 'GotoPrevMarker'     :  "",
"         \ 'GotoNextMarkerAny'  :  "",
"         \ 'GotoPrevMarkerAny'  :  "",
"         \ 'ListLocalMarks'     :  "m/",
"         \ 'ListLocalMarkers'   :  "m?"
"         \ }


" ===
" === Undotree 文件版本历史
" ===
noremap ty :UndotreeToggle<CR>
let g:undotree_DiffAutoOpen = 0

" ===
" === xkbswitch
" ===
let g:XkbSwitchEnabled = 1


" ===
" === Compile function
" ===
  noremap ss :call CompileRunGcc()<CR>
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
    silent! exec "!clear"
    exec "!time python3 %"
  elseif &filetype == 'html'
    exec "!firefox % &"
  elseif &filetype == 'markdown'
    exec "MarkdownPreview"
  elseif &filetype == 'vimwiki'
    exec "MarkdownPreview"
  endif
endfunc


" ___   _
"|_ _| | |    _____   _____  __      ____  ____  __
" | |  | |   / _ \ \ / / _ \ \ \ /\ / /\ \/ /\ \/ /
" | |  | |__| (_) \ V /  __/  \ V  V /  >  <  >  <
"|___| |_____\___/ \_/ \___|   \_/\_/  /_/\_\/_/\_\


```



### （2）插件管理

##### （2-0）插件管理器

+ 有啥：

  1. vim-plug

  2. Vundle

  3. Pathogen

  4. Dein.Vim

  5. Volt

+ 必须先安装：

  `https://github.com/junegunn/vim-plug`

  `curl -flo ~/.vim/autoload/plug.vim--create-dirs https://github.com/junegunn/vim-plug`

+ 如何寻找需要的插件？

  + 现有需求，后有插件；
  + 寻找：

    1. Google搜索关键词；
    2. 大部分插件托管在了github上；
    3. `https://vimawesome.com/`
    4. 浏览网上开源的vim配置借鉴想要的插件；

##### （2-1）美化插件

+ 启动界面：‘mhinz/vim-startify’
+ 状态栏美化：‘vim-airline/vim-airline’
+ 增加代码缩进线条：’Yggdroot/indenLline’
+ vim配色方案：
  1. `'w0ng/vim-hybrid'`
  2. `'altercation/vim-colors-solarized'`
  3. `'morhetz/gruvbox'`

##### （2-2）文件管理

+ 文件管理器`'scrooloose/nerdtree'`
+ 快速搜索文件`'ctrlpvim/ctrlp.vim'`

##### （2-3）模糊搜索、快速定位、成对修改、搜索替换

1. `junegunn/fzf.vim`

   + fzf是一个强大的命令行模糊搜索工具，fzf.vim集成到了vim里

   + 安装：

     + If you already installed fzf using Homebrew：

       ```
       Plug '/usr/local/opt/fzf'
       Plug 'junegunn/fzf.vim'
       ```

     + if you want to install fzf as well using vim-plug：

       ```
       Plug 'junegunn/fzf', { 'dir': '~/.fzf', 'do': './install --all' }
       Plug 'junegunn/fzf.vim'
       ```

   + Ag[Pattern]：模糊搜索字符串

   + Files[Path]：模糊搜索目录

2. `easymotion/vim-easymotion`

3.  `tpope/vim-surround`

    + ds（delete a surrounding）
    + cs（change a surrounding）
    + ys（you add a surrounding）

4. `brooth/far.vim`

   + 批量搜索替换
   + 重构代码的时候经常用到
   + 替换路径支持正则表达式
   + 格式：[][][命令] [搜索词] [替换词] [替换路径及文件]
   + `:Far foo bar **/*.py`
   + `:Fardo`

##### （2-4）golang语言编写相关

+ `Plug 'fatih/vim-go', { 'do': ':GoUpdateBinaries' }`

##### （2-5）Python语言编写相关

+ `Plug 'python-mode/python-mode', { 'for': 'python', 'branch': 'develop' }`

##### （2-6）代码浏览

+ 代码大纲tagbar：

  1. 构建 [Universal Ctags](https://ctags.io/)

  2. 【Docs】--》【BuildingCtags】--》【Building on MacOS】

  3. homebrew安装

     ```
     brew tap universal-ctags/universal-ctags
     brew install --HEAD universal-ctags
     ```

  4. Plug 'majutsushi/tagbar’

+ 高亮感兴趣的单词：

  + `'lfv89/vim-interestingwords'`

##### （2-7）代码补全

1. YouCompleteMe
2. deoplete.nvim
   + `'shougo/deoplete.nvim'`
3. coc.vim：
   + 异步补全
   + 多语言插件支持
   + 前端大神赵启明主导开发
   + neovim/vim8
   + LSP支持（full Language Server Protocol support as VSCode）
   + `'neoclide/coc.nvim'`
4. UltiSnips：

##### （2-8）代码格式化与静态检查

1. Neoformat：
   + `'sbdchd/neoformat'`
   + 需要安装对应语言格式化的库：
     1. autopep8（Python）
     2. pretties（js）
     3. goformat（golang）
   + `pip3 isntall autopep8`

2. vim-autoformat
3. ale：
   + `'w0rp/ale'`
   + 需要安装对应语言的lint库：
     1. eslint
     2. pylint
     3. golint
   + vim8/neovim支持异步检查，不会影响vim编辑
4. neomake

##### （2-9）代码注释

1. `'tpope/vim-commentary'`
   + gc注释和取消注释
   + 根据不同文件类型使用不同注释：

     1. \#（Python）
     2. //（golang）
     3. “（vimscript）

2. `'scrooloose/nerdcommenter'`



##### （2-10）vim和Git

1. `'tpope/vim-fugitive'`
   + Gedit, Gdiff, Gblame, Gcommit等命令
   + 也可以用tmux新开一个窗口来使用git

2. `'airblade/vim-gitgutter'`
   + 在vim里显示文件变动
   + 当我们修改文件之后可以显示当前文件的变动；
   + 哪些行新增，哪些行修改了，哪些行删除了；

3. `'junegunn/gv.vim'`
   + 在命令行查看提交记录；
   + GV命令调用
   + 浏览代码提交变更

##### （2-11）Vim and Tmux

##### （2-12）书签管理-标记及跳转

##### （2-13）中文输入问题

##### （2-14）Markdown

##### （2-15）Latex





### （3）基本命令

参考：

1. [PegasusWang](https://www.imooc.com/learn/1129)
2. [vimawesome](https://vimawesome.com/)



##### （3-1）Insert模式：

+ a i o A I O
+ Ctrl：
  + Crtl + h			           【Backspace】
  + Crtl + w			          【删除一个词】
  + Crtl + u			           【删除一行】    				
  + Crtl + c / Crtl + [ 		【Esc】
+ gi：快速跳转到你上次编辑的地方并进入Insert模式
+ inoremap：设置在insert模式下键盘映射



##### （3-2）Normal模式 -- 移动：

1. 方向移动：

   +  h  j  k  l
   +  H  J  K  L
2. 行内按单词移动：

   + w               【移到下一个word头】
   + W              【移到下一个WORD头】
   + e               【移到下一个word尾】
   + E               【移到下一个WORD尾】
   + word         【以非空白符分隔的单词】
   + WORD      【以空白符分隔的单词】
   + b               【回到上一个word开头】
   + B               【回到上一个WORD开头】
   + ge
3. 行内搜索跳转：

   + ;             【搜索该行下一个】
   + ,             【搜索该行上一个】
   + f
   + F           【反过来搜前面的字符】
   + t            【移动到搜索字符的前一个字符】
   + T
4. 行内水平移动：

   + `0`			【移动到行首（包括空白字符）】
   + `gm`		 【移动到行尾（包括空白字符）】
   + `^`			【移动到行首第一个非空白字符】
   + `$`			【移动到行尾最后一个非空白字符】
   + `g_`		   【移动到行尾非空白字符】
   + `n + |`	   【column n of current line】
5. 行间垂直移动：

   + ` )`			【句子间移动】
   + `(`			
   + `{`			【段落间移动】
   + `}`
   + easy-motion插件
6. 代码块移动：
   + [[
   + ][
   + [{
   + ]}
7. 注释内移动：
   + [/
   + ]/
8. 页面移动：

   + `gg`		         【文件开头】
   + `G`			        【文件结尾】
   + `gi`
   + `gf`    【（go to file）打开光标处的文件名】
   + `No. + G`	    【指定文件行数跳转】
   + `No. + %`	     【go to percentage n of file】
   + `H`			         【head：移动光标至屏幕开头】
   + `M`			         【middle：移动光标至屏幕中间】
   + `L`			         【lower：移动光标至屏幕结尾】
   + `Ctrl+y`
   + `Ctrl+e`
   + `Ctrl + b`	    【向上翻一页】
   + `Ctrl + u`	    【向上翻1/2页】
   + `Ctrl + d`	    【向下翻1/2页】
   + `Ctrl +f`	     【向下翻一页】
   + ` zz`			      【移动光标所在行至屏幕--中间】
   + ` zt`			      【移动光标所在行至屏幕--顶端】
   + `zb`		          【移动光标所在行至屏幕--底端】
9. 跳转：
   + `Ctrl + o`                【快速返回上次移动跳转】
   + `Ctrl+]`
   + %



##### （3-3）Normal模式 -- 增删改查：

1. 增加字符：

   + a  i  o  A  I  O

2. 删除字符：

   + x：删除光标所在处的字符
   + d：
     + dd
     + No. + dd
     + diw
     + daw    【删除一个单词包含空格】
     + d + i + ”
     + d + i + (
     + d + i + {
     + d + i +[
     + d + t + <字符>    【删除xxx，直到某个字符为至】
     + d + $
     + d + 0
     + d + No. + <方向键>
     + d + f + <搜索字符>

3. 修改字符：

   + r：
     + r    替换一个字符
     + R   不断替换后一个字符
   + s：
     + s    【删除字符并插入】
     + S   【删除行并插入】
     + No. + s
   + c：
     + `cc`	【改变整行（先删除整行，再在行首插入）】
     + `S`      【改变整行（先删除整行，再在行首插入）】
     + `ciw`
     + `ci"`
     + `ct:`
     + `c + f + <搜索字符>` 
     + `C`    【修改至行末】
   + `~`    --    大小写切换

4. 查询字符：

   + `/`	 【向后搜索】  
   + `?`	 【向前搜索】
   + `n`	 【查找下一处】
   + `N`	 【查找上一处】
   + `*`	 【当前单词前向匹配】
   + `#`	 【当前单词后向匹配】



##### （3-4）Normal模式  --  其他：

1. 保存和退出：
   + :w
   + :q
   + ZZ | :wq | :x【保存退出】
   + ZQ | :q!【不保存退出】
   + :w [filename]【另存为】
2. `u`：【撤销】
3. `<Ctrl+r>`：【取消撤销】
4. `Ctrl + a`：【数字自动加1】
   + `Ctrl + a`
   + `Ctrl + x`                    【数字自动减1】
   + `number + Ctrl + a `   【数字自动增加number】
   + `number + Ctrl + x `   【数字自动减少number】
5. 状态统计：
   + Ctrl+g【当前行信息】
   + g Ctrl+g【字数统计】
6. 



##### （3-5）Visual模式：

1. `v`    --    普通可视模式
2. `V`    --    整段选择
3. `Ctrl + v`    --    可视化块模式
4. `U`    --    选中的文本大写
5. `u`    --    选中的文本小写



##### （3-6）Command模式：

1. 设置vim：
   + :set sutoindent【自动缩进】
   + :set nu!【显示行号】
   + :set hlsearch【查找结果高亮显示】
   + :set warp【自动换行】
   + :set incsearch【立即显示当前输入匹配的】
   + :set ignorecase【忽略大小写】
   + :syntax enable【语法高亮】
2. :gui【以gvim模式打开】



##### （3-7）复制yank、粘贴put、寄存器

+ yy
+ d
+ p
+ P



##### （3-8）搜索替换：

+ 格式`:[range] s/{pattern}/{string}/[flags]`

  1. [range]
     + 表示范围；
     + `:10,20`      表示10-20行
     + %                表示全部
  2. s    --    substitute
  3. {pattern}    --    替换的模式
  4. {string}    --    替换的文本
  5. `[flags]`
     + 替换标志位
     + g（global），表示全局范围内执行；
     + c（confirm），可以确认或者拒绝修改；
     + n（number），报告匹配到的次数而不替换，用来查询匹配次数；

+ 支持正则表达式：

  + `:% s/plug/zhaobin/g`
  + `:20,50 s/duck/wxx/c`
  + `:% s/plug//n`    全文统计“plug”这个单词的个数
  + `% s/\<quack\>/zb/g`   正则表达式精确替换，只替换quack这个词，类似的do_quack则不会被替换；

+ 批量文件替换插件：

  Plug 'brooth/far.vim'



### （4）高级：

##### （4-1）vim宏

+ 从一个需求说起：给多行url链接加上双引号，怎么办？

+ 宏操作：

  ```
  1. 录制宏，并给宏指定寄存器
  qa
  
  2. 执行宏操作
  I"<Esc>A"<Esc>
  
  3. 结束录制
  q
  
  4. 选中要执行宏的区域
  V+G
  
  5. visual模式下对每行执行播放宏操作
  :'<,'>normal @a
  ```

+ visual模式自带宏操作：

  ```
  1. 进入Visual全选行
  V
  
  2. 用visual的自带宏
  :'<,'>normal I"
  
  3. 此时所有行的行首都加入了"，同样的方式应用于行尾
  
  4. 同理：
  V
  :'<,'>normal A"
  ```



##### （4-2）vim补全

+ 补全方式：

  + `<C-n>`                      【普通关键字】
  + `<C-x><C-n>`            【当前缓冲区关键字】
  + `<C-x><C-i>`            【包含文件关键字】
  + `<C-x><C-]>`            【标签文件关键字】
  + `<C-x><C-k>`            【字典查找】
  + `<C-x><C-l>`            【整行补全】
  + `<C-x><C-f>`            【文件名补全】
  + `<C-x><C-o>`            【全能（Omni）补全】

+ 常见三种类型补全：

  1. `C-n`  `C-p`

     ​		【补全单词】

  2. `C-x`  `C-f`

     ​		【补全文件名及文件路径】

  3. `C-x`  `C-o`

     ​		【补全代码】，需要开启文件类型检查，并安装插件

+ 骚操作：

  `:r! echo %`    插入当前文件名

  `:r! echo %:p`    插入当前文件的绝对路径

##### （4-3）多文件操作

1. Buffer：

   + 是啥：
     1. 【打开的一个文件的内存缓冲区】
     2. vim打开一个文件后会加载文件内容到缓冲区
     3. 之后的修改都是针对内存中的缓冲区，并不会直接保存到文件
     4. 直到我们执行`:w`的时候才会把修改内容写入到文件里
   + Buffer切换：
     1. `:ls`    列举当前缓冲区
     2. `:b buffer_No.`    跳转到第n个缓冲区
     3. 善用tab键补全！
     4. `:bpre`
     5. `:bnext`
     6. `:bfirst`
     7. `:blast`
     8. `:b buffer_name`

   

2. Window：

   + 是啥：
     1. 【window -- buffer可视化的分隔区域】
     2. 一个缓冲区可以分割成多个窗口；
     3. 每个窗口也可以打开不同的缓冲区；
     4. 每个窗口可以继续被无限分割；
   + 窗口分割：
     + `<Ctrl+w>s`    水平分割
     + `:sp`
     + `<Ctrl+w>v`    垂直分割
     + `:vs`
   + 切换窗口：
     1. `<C-w>w`        窗口间循环切换
     2. `<C-w>h`        切换到左边的窗口（已改键位`<mapleader>h`）
     3. `<C-w>j`
     4. `<C-w>k`
     5. `<C-w>l`
   + 重新排列窗口，设置窗口大小：
     1. `<C-w>H`        将窗口移到左边
     2. `<C-w>J`
     3. `<C-w>K`
     4. `<C-w>L`
     5. `<C-w>=`        【使所有窗口等宽、登高】
     6. `<C-w>_`        【最大化活动窗口的高度】
     7. `<C-w>|`        【最大化活动窗口的宽度】
     8. `[N]<C-w>_`   【把活动窗口的高度设为N行】
     9. `[N]<C-w>|`   【把活动窗口的宽度设为N行】

   

3. Tag：

   + 是啥？
     1. tba -- 可以组织窗口为一个工作区
     2. vim的Tab和其他编辑器的不太一样，可以想象成Linux的虚拟桌面；
     3. 比如一个Tab全用来编辑Python文件，这个Tab下可以有三个window，两个Buffer；另一个Tab全是HTML文件，开了两个Window，四个Buffer；
     4. 相比窗口，Tab一般用的比较少，Tab太多管理起来也比较麻烦，可以打开多个终端代替Tab功能；
   + 操作：
     1. `:tabe [dit] {filename}`           【新标签页中打开 {filename}】
     2. `:tabe`        【打开新的标签页】
     3. `:-tabnext`    【移到到左边的标签页】
     4. `:+tabnext`
     5. `<C-w>T`                                         【把当前窗口移到一个新标签页】
     6. `:tabc[lose]`                    【关闭当前标签页及其中的所有窗口】
     7. `:tabo[nly]`                      【只保留活动标签页，关闭所有其他标签页】

   

##### （4-4）更改主题及配色：

1. 系统自带：

   `:colorscheme<Enter>`        显示当前的主题配色，默认是default

   `:colorscheme <Ctrl+d>`    显示所有配色

   `:colorscheme 配色名<Enter>`    修改配色

2. 仓库下载：

   ```shell
   $ cd ~
   $ git clone https://github.com/flazz/vim-colorschemes.git
   $ git clone https://github.com/w0ng/vim-hybrid.git
   
   $ makdir .vim/colors
   $ cp vim-hybrid/colors/hybrid.vim ./.vim/colors
   
   $ vim 任意一个文件
   :colorscheme hybrid
   ```

3. 插件：



### （5）vim开发：

##### （5-1）text object（灵魂）

+ 介绍：
  1. 类似于面向对象编程；
  2. vim里对象的概念：一个单词，一段句子，一个段落；
  3. 其他编辑器只能操作单个字符来修改文本；
  4. 通过文本对象修改要比只操作当个字符高效；
+ `[number]<command>[text object]`
  + number：表示次数
  + command：命令
    1. d （delete）
    2. c （change）
    3. y （yank）
    4. v （Visual模式）
  + text object：要操作的文本对象
    1. w 单词
    2. s 句子
    3. p 段落
    4. ()
    5. []
    6. {}
    7. <>
    8. “”
    9. ‘’
    10. ``
+ 情况：
  + iw        【inner word，选中当前单词】
  + aw       【around word，选中当前单词，以及其后的空格】
  + iW        【W是以空格来分割的单词】
  + aW
  + is
  + as
  + ip
  + ap
  + f<字符>



#####（5-2）vimscript函数

《笨方法学Vimscript》



### （6）neovim

