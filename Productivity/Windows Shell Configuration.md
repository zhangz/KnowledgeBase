# [Cmder](http://cmder.net/)配置

## 添加到右键菜单

在管理员权限的终端输入以下语句即可：

    cmder.exe /REGISTER ALL

## 解决文字重叠问题

<kbd>Win</kbd> + <kbd>ALT</kbd> + <kbd>P</kbd> 唤出设置界面 > main > monospace，去掉勾选即可。

## 修改命令提示符号

Cmder预设的命令提示符号是λ。可以改成Mac/Linux环境下常见的$符号。
编辑安装目录下的vendor\init.bat文件(min版本15行)，把：

    @prompt $E[1;32;40m$P$S{git}{hg}$S$_$E[1;30;40m{lamb}$S$E[0m

修改成以下即可：

    @prompt $E[1;32;40m$P$S{git}{hg}$S$_$E[1;30;40m$$$S$E[0m


## 自定义alias

* 在终端输入`alias`可以查看。
* 自定义aliases：打开Cmder目录下的config文件夹，里面的aliases文件就是我们可以配置的别名文件。

## Assign different color scheme for different commands
Select Features/App distinct, then add application distinct settings which override the default color palette.

## Integrate Sublime Text
1. Create a new folder called 'Sublime Text' in the `/cmder/vendor/` location.
2. Download a portable version of Sublime Text from their website and unzip the content into the newly created folder.
3. In the `aliases` file under `/cmder/config/`,add:
     
		subl="%CMDER_ROOT%\vendor\Sublime Text\sublime_text.exe" $1 -new_console:s75V

4. Restart and type `subl foo.txt` will open foo.txt in current directory. If the file doesn't exist, Sublime Text will create it.
5. To quickly exit Sublime, you just press Alt+F+X. And you're back to your cmd.
6. To make Sublime Text not remember your previously opened files, in Sublime, under Preferences-->Settings - User, put:

		"hot_exit": false,
    	"remember_open_files": false

The `-new_console:s75V` will split the console horizontally where sublime will take up 75% of the view. You can also change the split behavior. Here are some other examples:

- Split will be created to the right, both splits becomes 50%.

		-new_console:s or -new_console:s50H

- Split will be created horizontally, both splits becomes 50%. 

		-new_console:s50V

- Replace the entire screen with sublime.

		-new_console
		

## Keyboard shortcuts

### Tab manipulation

* <kbd>Ctrl</kbd> + <kbd>T</kbd> : New tab dialog (maybe you want to open cmd as admin?)
* <kbd>Ctrl</kbd> + <kbd>W</kbd> : Close tab
* <kbd>Ctrl</kbd> + <kbd>D</kbd> : Close tab (if pressed on empty command)
* <kbd>Shift</kbd> + <kbd>Alt</kbd> + <kbd>#Number</kbd> : Fast new tab: <kbd>1</kbd> - CMD, <kbd>2</kbd> - PowerShell
* <kbd>Alt</kbd> + <kbd>Enter</kbd>: Fullscreen

### Shell

* <kbd>Shift</kbd> + <kbd>Up</kbd> : Traverse up in directory structure (lovely feature!)
* <kbd>End</kbd>, <kbd>Home</kbd>, <kbd>Ctrl</kbd> : Traversing text with as usual on Windows
* <kbd>Ctrl</kbd> + <kbd>R</kbd> : History search
* <kbd>Shift</kbd> + Mouse : Select and copy text from buffer



# [Clink](https://mridgers.github.io/clink/)配置

## 自動完成功能
輸入命令到一半時，輸入<kbd>Tab</kbd>會自動顯示可能的命令清單，所有在PATH環境變數下的那些目錄全部都會搜尋出來

## 查詢歷史指令
按下<kbd>Ctrl+R</kbd>就可以開始搜尋之前輸入過的指令

## 基本行內編輯功能
- Ctrl + ←：鍵盤游標可向左移動一個字
- Ctrl + →：鍵盤游標可向右移動一個字
- Ctrl + A：鍵盤游標直接移到最左邊（最前面）
- Ctrl + E：鍵盤游標直接移到最右邊（最後面）
- Ctrl + H：等同於 Backspace 按鍵
- ESC：清除目前輸入的整行

## 支援多種鍵盤快速鍵使用
- Ctrl + V：貼上剪貼簿中的文字內容
- Ctrl + Z：復原輸入 (Undo)
- Ctrl + Alt + U：回上一層 (等於幫你輸入了 cd .. 命令，超方便的)
- Alt + H：顯示所有快速鍵清單

## Clink可以直接跟原生的cmd.exe搭配使用
- 執行 `vendor\clink\clink.bat` 即可啟用Clink功能。
- 以系統管理員身分執行 `vendor\clink\clink.bat autorun -i` 可將clink自動注入到Cmd.exe之中。
- 如要移除Clink綁定，可執行 `vendor\clink\clink.bat autorun -u` 即可。

## Other Tools
- https://github.com/bmatzelle/gow
- http://babun.github.io/
- https://github.com/milkbikis/powerline-shell

http://ju.outofmemory.cn/entry/257660
http://www.rxna.cn/post/wiki/babun-pei-zhi