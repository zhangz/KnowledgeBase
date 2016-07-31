
### Office 2010使多个Excel文件在独立的窗口中打开
1.打开regedit编辑注册表。
2.定位到【HKEY_CLASSES_ROOT\Excel.Sheet.12\shell\Open】，展开Open，将ddeexec删除，然后选中command，双击右侧窗格的"默认"，将末尾的/dde改为"%1"（注意有双引号）；再双击command，同样将末尾的/dde改为"%1"。  定位到【HKEY_CLASSES_ROOT\Excel.Sheet.8\shell\Open】，做和上面一样的改动。