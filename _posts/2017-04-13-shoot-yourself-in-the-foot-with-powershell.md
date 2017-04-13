---
layout: post
title:  "Shoot yourself in the foot with powershell"
date:   2017-04-13 14:13:00 -0200
categories: powershell powerfoot
---

I've manged to *shot myself in the foot* in powershell twice lately, therefore I've figured I could write down those examples as a warning to future self (and humanity in general).

Without further ado, here they are:

## Strong typing can backfire right at you

```powershell
[string]$x = 'foo'
$x = ($x.ToLower() -eq 'bar')
Write-Output $x
if ($x)
{
    Write-Output "foobar"
}
```

Writes to console:
```
False
foobar
```

## The exit codes..

..are interesting, especially if you want to support PS2.0 and use negative values

Apparently, at Win7 times someone at Microsoft thought storing exit codes as unsigned int was a good idea.

```powershell
PS C:\> Write-Output "exit -2" > test.ps1
PS C:\> powershell -Command '.\test.ps1'
PS C:\> echo $LASTEXITCODE
1
PS C:\> powershell -Command '.\test.ps1; exit $LASTEXITCODE'
PS C:\> echo $LASTEXITCODE
-2
PS C:\> powershell -Version 2.0 -Command '.\test.ps1; exit $LASTEXITCODE'
PS C:\> echo $LASTEXITCODE
65534
```

***

I have a strange feeling that this collection will grow bigger eventually..
