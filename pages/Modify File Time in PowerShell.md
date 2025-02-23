- Modify File Time in PowerShell #PowerShell
	- For a file:
		- For example, if the file name is `FileName.txt` and the time to modify is `2023-07-08 13:53:11`, you can use the following code.
		  
		  ```powershell
		  (ls "FileName.txt").CreationTime="2023-07-08 13:53:11"
		  (ls "FileName.txt").LastWriteTime="2023-07-08 13:53:11"
		  (ls "FileName.txt").LastAccessTime="2023-07-08 13:53:11"
		  ```
	- For a folder:
		- ```powershell
		  (Get-ItemProperty "FolderName").LastWriteTime="2023-07-08 13:53:11"
		  ```
	- For all files inside the folder (including those in subfolders and their contents):
		- ```powershell
		  Get-ChildItem -recurse | ForEach-Object { $_.LastWriteTime="2023-07-08 13:53:11" }
		  ```
	- Reference:
		- [about Properties - PowerShell | Microsoft Learn](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_properties?view=powershell-7.3)