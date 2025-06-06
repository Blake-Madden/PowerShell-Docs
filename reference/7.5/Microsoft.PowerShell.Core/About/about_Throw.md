---
description: Describes the `throw` keyword, which generates a terminating error.
Locale: en-US
ms.date: 08/24/2022
online version: https://learn.microsoft.com/powershell/module/microsoft.powershell.core/about/about_throw?view=powershell-7.5&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Throw
---
# about_Throw

## Short description

Describes the `throw` keyword that generates a terminating error.

## Long description

The `throw` keyword causes a terminating error. You can use the `throw` keyword
to stop the processing of a command, function, or script.

For example, you can use the `throw` keyword in the script block of an `if`
statement to respond to a condition or in the `catch` block of a
`try`-`catch`-`finally` statement.

The `throw` keyword can throw any object, such as a user message string or the
object that caused the error.

## Syntax

The syntax of the `throw` keyword is as follows:

```powershell
throw [<expression>]
```

The expression in the `throw` syntax is optional. When the `throw` statement
doesn't appear in a `catch` block, and it doesn't include an expression, it
generates a **ScriptHalted** error.

```powershell
throw
```

```Output
Exception: ScriptHalted
```

If the `throw` keyword is used in a `catch` block without an expression, it
throws the current RuntimeException again. For more information, see
[about_Try_Catch_Finally](about_Try_Catch_Finally.md).

## Throwing a string

The optional expression in a `throw` statement can be a string, as shown in the
following example:

```powershell
throw "This is an error."
```

```Output
Exception: This is an error.
```

## Throwing other objects

The expression can also be an object that throws the object that represents the
PowerShell process, as shown in the following example:

```powershell
throw (Get-Process pwsh)
```

```Output
Exception: System.Diagnostics.Process (pwsh) System.Diagnostics.Process (pwsh) System.Diagnostics.Process (pwsh)
```

You can use the **TargetObject** property of the **ErrorRecord** object in the
`$Error` automatic variable to examine the error.

```powershell
$Error[0].TargetObject
```

```Output
 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
    125   174.44     229.57      23.61    1548   2 pwsh
     63    44.07      81.95       1.75    1732   2 pwsh
     63    43.32      77.65       1.48    9092   2 pwsh
```

You can also `throw` an **ErrorRecord** object or a .NET exception. The
following example uses the `throw` keyword to throw a
**System.FormatException** object.

```powershell
$formatError = New-Object System.FormatException
throw $formatError
```

```Output
OperationStopped: One of the identified items was in an invalid format.
```

## The resulting error

The `throw` keyword can generate an **ErrorRecord** object. The **Exception**
property of the **ErrorRecord** object contains a **RuntimeException** object.
The remainder of the **ErrorRecord** object and the **RuntimeException** object
varies depending on the object thrown.

The `throw` object is wrapped in an **ErrorRecord** object, and the ErrorRecord
object is automatically saved in the `$Error` automatic variable.

## Using `throw` to create a mandatory parameter

Unlike past versions of PowerShell, don't use the `throw` keyword for parameter
validation. See
[about_Functions_Advanced_Parameters](about_Functions_Advanced_Parameters.md)
for the correct way.

## See also

- [about_Break](about_Break.md)
- [about_Continue](about_Continue.md)
- [about_Scopes](about_Scopes.md)
- [about_Trap](about_Trap.md)
- [about_Try_Catch_Finally](about_Try_Catch_Finally.md)
