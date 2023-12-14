# Code Page

[Список](https://pygments.org/docs/lexers/) доступных наименований языков и форматов файлов.

Примеры отображения кода на разных языках:

### Bash

код с нумерацией начиная с 1

```bash linenums="1"


#!/bin/bash 

name=$1

case "$name" in
  "Vasya" )
    nameString="Vasiliy"
	greetString="Whatsup"
  ;;
  "Masha" )
	greetString="Hey"
	nameString="Maria"
  ;;
  * )
	greetString="Hello"
	nameString="stranger"
  ;;
esac

echo "$greetString, $nameString!"
```

### Powershell

код с нумерацией начиная с 1, строки 2-5 8-10 выделены

```powershell linenums="1" hl_lines="2-5 8-10"

#
# Script module for module 'PackageManagement'
#
Set-StrictMode -Version Latest
Microsoft.PowerShell.Utility\Import-LocalizedData  LocalizedData -filename PackageManagement.Resources.psd1

# Summary: PackageManagement is supported on Windows PowerShell 3.0 or later, Nano Server and PowerShellCore
$isCore = ($PSVersionTable.Keys -contains "PSEdition") -and ($PSVersionTable.PSEdition -ne 'Desktop')
$binarySubPath = ''
if ($isCore)
{
    $binarySubPath = Join-Path -Path 'coreclr' -ChildPath 'netstandard2.0'
} else {
    $binarySubPath = 'fullclr'
}

# Set up some helper variables to make it easier to work with the module
$script:PSModule = $ExecutionContext.SessionState.Module
$script:PSModuleRoot = $script:PSModule.ModuleBase

$script:PkgMgmt = 'Microsoft.PackageManagement.dll'
$script:PSPkgMgmt = 'Microsoft.PowerShell.PackageManagement.dll'


# Try to import the OneGet assemblies at the same directory regardless fullclr or coreclr
$OneGetModulePath = Join-Path -Path $script:PSModuleRoot -ChildPath $script:PkgMgmt
$binaryModuleRoot = $script:PSModuleRoot


if(-not (Test-Path -Path $OneGetModulePath))
{
    # Import the appropriate nested binary module based on the current PowerShell version
    $binaryModuleRoot = Join-Path -Path $script:PSModuleRoot -ChildPath $binarySubPath
    $OneGetModulePath = Join-Path -Path $binaryModuleRoot -ChildPath $script:PkgMgmt
}

$PSOneGetModulePath = Join-Path -Path $binaryModuleRoot -ChildPath $script:PSPkgMgmt
$OneGetModule = Import-Module -Name $OneGetModulePath -PassThru
$PSOneGetModule = Import-Module -Name $PSOneGetModulePath -PassThru


# When the module is unloaded, remove the nested binary module that was loaded with it
if($OneGetModule)
{
    $script:PSModule.OnRemove = {
        Remove-Module -ModuleInfo $OneGetModule
    }
}

if($PSOneGetModule)
{
    $script:PSModule.OnRemove = {
        Remove-Module -ModuleInfo $PSOneGetModule
    }
}

```

### Вывод данных сессии PowerShell    
```pwsh-session


Name               Host Version & Build Version    
----               ----------------------------    
esxi.test.com     VMware ESXi 7.0.2 build-17630552


PS C:\Users\admin1> 
```


### Python

```py title="Пример скрипта на Python"
import tensorflow as tf
def whatever()
```

### Swift
```swift
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// Prints "someInt is now 107, and anotherInt is now 3"
```

```swift
class Superclass {
    private var xValue = 12
    var x: Int {
        get { print("Getter was called"); return xValue }
        set { print("Setter was called"); xValue = newValue }
    }
}


// This subclass doesn't refer to oldValue in its observer, so the
// superclass's getter is called only once to print the value.
class New: Superclass {
    override var x: Int {
        didSet { print("New value \(x)") }
    }
}
let new = New()
new.x = 100
// Prints "Setter was called"
// Prints "Getter was called"
// Prints "New value 100"


// This subclass refers to oldValue in its observer, so the superclass's
// getter is called once before the setter, and again to print the value.
class NewAndOld: Superclass {
    override var x: Int {
        didSet { print("Old value \(oldValue) - new value \(x)") }
    }
}
let newAndOld = NewAndOld()
newAndOld.x = 200
// Prints "Getter was called"
// Prints "Setter was called"
// Prints "Getter was called"
// Prints "Old value 12 - new value 200"
```

## Yaml файл с аннотациями

``` yaml
theme:
  features:
    - content.code.annotate # (1)
```

1.  :man_raising_hand: I'm a code annotation! I can contain `code`, __formatted
    text__, images, ... basically anything that can be written in Markdown.


