---
description: The ISEFileCollection object is a collection of ISEFile objects.
ms.date: 03/27/2025
title: The ISEFileCollection Object
---

# The ISEFileCollection Object

The **ISEFileCollection** object is a collection of **ISEFile** objects. An example is the
`$psISE.CurrentPowerShellTab.Files` collection.

## Methods

### `Add( [FullPath] )`

Supported in Windows PowerShell ISE 2.0 and later.

Creates and returns a new untitled file and adds it to the collection. The **IsUntitled** property
of the newly created file is `$true`.

- **[FullPath]** - Optional string - The fully specified path of the file. An exception is generated
  if you include the **FullPath** parameter and a relative path, or if you use a file name instead
  of the full path.

```powershell
# Adds a new untitled file to the collection of files in the current PowerShell tab.
$newFile = $psISE.CurrentPowerShellTab.Files.Add()

# Adds a file specified by its full path to the collection of files in the current
# PowerShell tab.
$psISE.CurrentPowerShellTab.Files.Add("$PSHOME\Examples\profile.ps1")
```

### `Remove( File, [Force] )`

Supported in Windows PowerShell ISE 2.0 and later.

Removes a specified file from the current PowerShell tab.

**File** - String The ISEFile file that you want to remove from the collection. If the file hasn't
been saved, this method throws an exception. Use the **Force** switch parameter to force the removal
of an unsaved file.

**[Force]** - optional Boolean If set to `$true`, grants permission to remove the file even if it
hasn't been saved after last use. The default is `$false`.

```powershell
# Removes the first opened file from the file collection associated with the current
# PowerShell tab.  If the file hasn't yet been saved, then an exception is generated.
$firstfile = $psISE.CurrentPowerShellTab.Files[0]
$psISE.CurrentPowerShellTab.Files.Remove($firstfile)

# Removes the first opened file from the file collection associated with the current
# PowerShell tab, even if it hasn't been saved.
$firstfile = $psISE.CurrentPowerShellTab.Files[0]
$psISE.CurrentPowerShellTab.Files.Remove($firstfile, $true)
```

### `SetSelectedFile( selectedFile )`

Supported in Windows PowerShell ISE 2.0 and later.

Selects the file that's specified by the **SelectedFile** parameter.

**SelectedFile** - Microsoft.PowerShell.Host.ISE.ISEFile
The ISEFile file that you want to select.

```powershell
# Selects the specified file.
$firstfile = $psISE.CurrentPowerShellTab.Files[0]
$psISE.CurrentPowerShellTab.Files.SetSelectedFile($firstfile)
```

## See Also

- [The ISEFile Object][03]
- [Purpose of the Windows PowerShell ISE Scripting Object Model][01]
- [The ISE Object Model Hierarchy][02]

<!-- link references -->
[01]: Purpose-of-the-Windows-PowerShell-ISE-Scripting-Object-Model.md
[02]: The-ISE-Object-Model-Hierarchy.md
[03]: The-ISEFile-Object.md
