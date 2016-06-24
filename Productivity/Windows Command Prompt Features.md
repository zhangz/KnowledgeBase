[Command-line reference](https://technet.microsoft.com/en-us/library/bb490890.aspx)
http://ss64.com/nt/

**PIPE COMMAND OUTPUT TO THE CLIPBOARD**

In Vista they added clip.exe. It captures any standard input and puts in the clipboard.
That means you can

	dir /s | clip
	ver | clip
	ipconfig /all | clip

Here's another

	type myfile.txt | clip

`type` followed by the filename prints the contents of the file. Use the pipe (|) and clip to copy to the clipboard. Works in the same way `cat` does in PowerShell.

**Graphically displays the directory structure of a path**

	tree [Drive:][Path] [/f] [/a]

**Remote Desktop**

Remote desktop command line (use /console if all slots full)

	mstsc /v:SERVERNAME [/console]

List other RDP sessions on the server

	query session

Kick someone off -- useful when paired with sysinternals psexec, or powershell remoting

	logoff SESSIONID

**Displays the location of a file**

The `where` command displays the location of a file

	where clip.exe

Syntax

	WHERE [/r Dir] [/q] [/f] [/t] Pattern

	/r      A recursive search, starting with the specified Dir directory.
	/q      Don’t display the files but return either an exit code of 0 for success
            or 1 for failure.
	/f      Display the output file name in quotation marks.
	/t      Display the size, time stamp, and date stamp of the file.
	pattern  The Drive\Directory\file, or set of files to be found.
             you can use wildcard characters ( ? * ) and UNC paths.

e.g. `where /r d:\ test.exe`

**Pushd and Popd**

`pushd`: Stores the current directory for use by the `popd` command, then changing the current directory to the specified directory.

	pushd [Path]

Every time you use the `pushd` command, a single directory is stored for your use. However, you can store multiple directories by using the `pushd` command multiple times. The directories are stored sequentially in a virtual stack.

`popd`: Changes the current directory to the directory stored by the pushd command.
	
	popd

You can use the popd command to change the current directory to the directory most recently stored by the pushd command. If you use the popd command, the directory on the top of the stack is removed from the stack as the current directory is changed to that directory. 


**F7 GIVES YOU A GRAPHICAL (TEXT) HISTORY**

If you have already typed a few commands, you can press F7 to get an ANSI popup with a list of commands you've typed.

**TRANSPARENT COMMAND PROMPT**

After Windows 10, you can make the Command Prompt transparent!

**FULL SCREEN COMMAND PROMPT**

Pressing "ALT-ENTER" in the command prompt (any prompt, cmd, powershell, or bash) will make it full screen. I like to put my command prompt on another virtual desktop and then use CTRL-WIN-ARROWS to move between them.