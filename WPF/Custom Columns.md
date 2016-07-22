# Custom Columns
This is inveatigation on creating custom columns of WPF (WinForms) grids. The features below needs to be supported:
1. Dynamically add/remove columns at runtime.
2. Do it in MVVM style
3. When update a single column, only send its value and locate the column then update it. This means we do updates without sending all data (unchanged) to client from service.
4. Paging support
4. We used the ICustomTypeDescriptor interface for such a task but that will get messy fast.

## Approach 1
You can do it by implementing `ITypedList` interface by your ItemsSource collection. DataGrid will use `PropertyDescriptor`s returned from `ITypedList.GetItemProperties` method to generate `DataGridColumn`s automatically without any additional XAML by internally using `DataGrid.GenerateColumns` method.

Implementing dynamic properties using `PropertyDescriptor` only works when the client (the entity actually looking up, reading and writing properties) supports the `TypeDescriptor` way of doing things. This is sometimes called "the component model" and not all property consumers support it. For example WPF does support it, for example for databinding. Also, WinForms supports it. But in other places property are just properties and the component model is not used.

http://stackoverflow.com/questions/6166236/add-properties-at-runtime

https://github.com/zhangz/WPF-Helpers

http://www.reimers.dk/jacob-reimers-blog/auto-generating-datagrid-columns-from-dynamicobjects

https://jopinblog.wordpress.com/2007/04/30/implementing-itypedlist-for-virtual-properties/

https://jopinblog.wordpress.com/2007/05/12/dynamic-propertydescriptors-with-anonymous-methods/

https://blogs.msdn.microsoft.com/parthopdas/2006/01/04/understanding-the-typedescriptor-a-metadata-engine-for-designtime-code/

http://stackoverflow.com/questions/704724/programatically-add-column-rows-to-wpf-datagrid/8892340#8892340

https://web.archive.org/web/20120419005554/http://lucidsnippets.com/blog/dynamic-wpf-datagrid-columns-using-propertydescriptors

http://www.shujaat.net/2012/09/dynamically-generated-properties-using.html

## Approach 2
Use a `DependencyProperty` to bind a listview to an unknown dataset.
http://www.codeproject.com/KB/WPF/WPF_DynamicListView.aspx
http://stackoverflow.com/questions/320089/how-do-i-bind-a-wpf-datagrid-to-a-variable-number-of-columns

## Appraoch 3
http://paulstovell.com/blog/dynamic-datagrid

## Approach 4
You just can't bind a GridView control to an ExpandoObject instance (but you can do this to DataGrid). Use impromptu to convert the ExpandoObject to a POCO class.
https://github.com/zhangz/impromptu-interface

`ICustomTypeDescriptor` vs. 

http://www.codeproject.com/Articles/270972/Paging-Data-from-the-Server-with-Silverlight