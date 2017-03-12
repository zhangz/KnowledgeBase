## Package Control
https://sublime.wbond.net/installation

## Extensions
- ConvertToUTF8 (and Codecs33 for mac/linux)
- IMESupport
- BracketHighlighter
- Color Highlighter
- Emmet
- SideBarEnhancements
- Themr
- Anaconda
- Modific
- GitGutter
- GitSavvy
- GitStatusBar
- Python PEP8 Autoformat
- HTMLBeautify
- Alignment
- All Autocomplete

## Theme
- Material
- Tomorrow Night Italics Color Scheme

## User Settings

```
{
	"auto_complete": false,
	"close_windows_when_empty": true,
	"remember_open_files": false,
	"highlight_modified_tabs": true,
	"highlight_line": true,
	"draw_white_space": "all",
	"tab_size": 4,
	"translate_tabs_to_spaces": true,
	"trim_automatic_white_space": true,
	"trim_trailing_white_space_on_save": true,
	"detect_indentation" : false,
	"ensure_newline_at_eof_on_save": true,
	"find_selected_text": true,
	"fold_buttons": false,
	"folder_exclude_patterns":
	[
		".svn",
		".git",
		".hg",
		"CVS",
		"_build",
		"dist",
		"build",
		"site"
	],
	"hot_exit": false,
	"ignored_packages":
	[
		"Git",
		"GitSavvy",
		"RestructuredText",
		"SublimeLinter-flake8",
		"Vintage"
	],
        // Follow the PEP 8 recommendations of limiting docstrings to 72 characters and limiting all other lines to 79 characters.
        "rulers":
        [
            72,
            79
        ],
        "word_wrap": true,
	"word_wrap": true,
	"wrap_width": 80,
	"color_scheme": "Packages/Tomorrow Night Italics Color Scheme/Tomorrow-Night-Italics.tmTheme",
	"theme": "Material-Theme-Darker.sublime-theme",
	"material_theme_accent_orange": true,
	"material_theme_accent_scrollbars": true,
	"material_theme_appbar_orange": true,
	"material_theme_arrow_folders": true,
	"material_theme_bullet_tree_indicator": true,
	"material_theme_compact_sidebar": true,
	"material_theme_contrast_mode": true,
	"material_theme_small_statusbar": true,
	"material_theme_small_tab": true,
	"material_theme_tree_headings": false,
	"font_face": "YaHei Consolas Hybrid",
	"font_size": 12,
	"caret_style": "phase"
}
```

## Shortcuts Cheatsheet
#### 通用
- `Alt`：调出菜单
- `Ctrl + Shift + P`：调出命令板（Command Palette）
- `Ctrl + `` ：调出控制台

#### 编辑
- `Ctrl + Enter`: 在当前行下面新增一行然后跳至该行
- `Ctrl + Shift + Enter`: 在当前行上面增加一行并跳至该行
- `Ctrl + Shift + ↑/↓`: 移动当前行
- `Ctrl + /`: 代码注释
- `Ctrl + KK`: 从光标处删除至行尾
- `Ctrl + Shift + K`: 删除整行
- `Ctrl + K + Backspace`: 从光标处删除至行
- 多列编辑：按住`Ctrl`点击鼠标，会出现多个闪烁的光标，这时可同时修改多处
- `Ctrl + [` 向左缩进
- `Ctrl + ]` 向右缩进
- `Ctrl + Shift + V` 以当前缩进粘贴代码

#### 选择
- `Ctrl + L`: 选择整行
- `Ctrl + D`：选择当前光标所在的词并高亮该词所有出现的位置，再次 `Ctrl + D` 选择该词出现的下一个位置，在多重选词的过程中，使用 `Ctrl + K` 进行跳过，使用 `Ctrl + U` 进行回退，使用 `Esc` 退出多重编辑
- `Ctrl + Shift + L`: 将当前选中区域打散，然后进行同时编辑
- `Ctrl + J`: 可以把当前选中区域合并为一行
- `Ctrl + M`：在起始括号和结尾括号间切换
- `Ctrl + Shift + M`：快速选择括号间的内容
- `Ctrl + Shift + J`：快速选择同缩进的内容
- `Ctrl + Shift + Space`：快速选择当前作用域（Scope）的内容

#### 查找&替换
- `Alt + F3`: 选中关键字出现的所有位置
- `Ctrl + F/H`：进行标准查找/替换，之后：
    - `Alt + C`：切换大小写敏感（Case-sensitive）模式
    - `Alt + W`：切换整字匹配（Whole matching）模式
    - `Alt + R`：切换正则匹配（Regex matching）模式
    - `Ctrl + Shift + H`：替换当前关键字
    - `Ctrl + Alt + Enter`：替换所有关键字匹配
- `Ctrl + Shift + F`：多文件搜索&替换

#### 跳转
- `Ctrl + P`：跳转到指定文件，输入文件名后可以：
    - `@` 符号跳转：输入 `@symbol` 跳转到 `symbol` 符号所在的位置
    - `#` 关键字跳转：输入 `#keyword` 跳转到 `keyword` 所在的位置
    - `:` 行号跳转：输入 `:12` 跳转到文件的第`12`行
- `Ctrl + R`：跳转到指定符号
- `Ctrl + G`：跳转到指定行号
- `F12`: 跳转到代码定义

#### 窗口
- `Ctrl + Shift + N`: 创建一个新窗口
- `Ctrl + N`: 在当前窗口创建一个新标签
- `Ctrl + W`: 关闭窗口
- `Ctrl + Shift + T`: 恢复刚刚关闭的标签

#### 屏幕
- `Alt + Shift + 2`：进行左右分屏
- `Alt + Shift + 8`：进行上下分屏
- `Alt + Shift + 5`：进行上下左右分屏
- 分屏之后，使用 `Ctrl + 数字键` 跳转到指定屏，使用 `Ctrl + Shift + 数字键` 将当前屏移动到指定屏

## Ref
- https://www.kennethreitz.org/essays/sublime-text-3-heaven
- http://zh.lucida.me/blog/sublime-text-complete-guide/
- http://wdxtub.com/2016/03/24/sublime-guide/
- http://sublime-text-unofficial-documentation.readthedocs.io/en/latest/reference/keyboard_shortcuts_win.html
- http://sublime-text-unofficial-documentation.readthedocs.io/en/latest/reference/keyboard_shortcuts_osx.html
