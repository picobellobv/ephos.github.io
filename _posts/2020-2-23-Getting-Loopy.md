---
title: PowerShell 101 - Loops Foreach
teaser: Let's get loopy with foreach!
category: powershell
tags: [powershell, powershell_101]
published : false
reddit_post:
featured_comments:
---

## PowerShell 101 - Loops

### Table of Contents

- [PowerShell 101 - Loops](#powershell-101---loops)
  - [Table of Contents](#table-of-contents)
  - [About PowerShell 101](#about-powershell-101)
  - [Let's Talk About Loops](#lets-talk-about-loops)
  - [Foreach](#foreach)
    - [foreach](#foreach-1)
    - [ForEach-Object](#foreach-object)
    - [Foreach-Object -Parallel](#foreach-object--parallel)
    - [.ForEach()](#foreach-2)
  - [Stay Tune for More Loop Action](#stay-tune-for-more-loop-action)

### About PowerShell 101

Welcome to a new series that I am unoriginality calling _"PowerShell 101"_!  This will be similar to the _["Did You Know?"][DidYouKnow]_ series, but focusing on entry level topics for folks that want a refresh and those just starting out!

Onto the fun!

### Let's Talk About Loops

There is a lot of documentation and blog posts out there on this topic, my hope and intent is that this helps newcomers who may just be starting their coding, scripting, and/or PowerShell journey.  Loops are language constructs you'll likely bump into very early on.  Knowing how to use them and which ones to use when can be overwhelming with so many options.  

I will attempt to link actual PowerShell documentation pertinent to each loop at the bottom of its section, so be sure to look for those. Let's dive into them!

### Foreach

This is my favorite type of loop, and tends to be the one I use and write the most.  Thi is where we will start!  

With that said, there are different variants of **"ForEach"**.  This confused the ever loving heck out of me when I started learning PowerShell, what is the difference between `foreach`, `ForEach`/ `ForEach-Object`, and `.foreach()`???

Keep reading to find out 😉...

#### foreach

This is the **foreach** *keyword*.  I intentionally lower cased it as I believe keywords should be lowered cased *[(insert code style religious war here)][keywords]*.  

Generally in the wild I find it use primarily used in functions and scripts, though you can still also use it interactively in the terminal though in that context it might be a bit awkward.  

**foreach** takes an entire collection of objects and iterates through them.  It allows you define a variable that you can use to manipulate an item as you process them.  For example lets look at the following...

```powershell
foreach ($item in $items) {
  Write-Output -InputObject "Current item: $item"
}
```

The code above assumes you have a collection of objects stored in the `$items` variable, it will iterate through the collection and you can take some action on each `$item` in the script block _(as identified by the `{ }`)_.  When you declare a `foreach ($item in $collection)` , you can choose any variable name you'd like to use inside the script block.  

In the example above we used `$item` to identify the current item we are working with in the `$items` collection.  You could have also used `$i` or `$myCurrentItem` or `$supercalifragilisticexpialidocious`.  You get the point though, you choose the variable name you want to use to access the current item you are working with in the `foreach` loop.

Using `foreach` in a slightly more practical sense below, we'll iterate through a collection of objects we have from `Get-Process`.  This gets all the processes running on the machine and if the CPU is greater than 0 it will output some text like _"Process pwsh-preview using 1.22 CPU!"_

```powershell
$processes = Get-Process
foreach ($process in $processes) {
  if ($process.CPU -gt 0) {
    Write-Output -InputObject "Process $($process.Name) using $($process.CPU) CPU!"
  }
}
```

**PowerShell Help Page:** [about_Foreach][about_Foreach]

#### ForEach-Object

```powershell
Get-Process | ForEach-Object -Process {$_.Name}
# Same as...
Get-Process | ForEach -Process {$_.Name}
# Same as...
Get-Process | ForEach {$_.Name}
# Same as...
Get-Process | Foreach-Object Name
```

**PowerShell Help Page:** [ForEach-Object][ForEach-Object]

#### Foreach-Object -Parallel

From the folks who brought you PowerShell, comes the latest an

#### .ForEach()

The `.ForEach()` method was added in PowerShell v4.0

```powershell
$process = Get-Process
$process.ForEach('$_.Name')
```

**PowerShell Help Page:** [about_Arrays][about_Arrays]

### Stay Tune for More Loop Action

We still need to cover...

- For
- While
- Do
  - Do While
  - Do Until

**<span style="color:orange">Caution:</span>**

[DidYouKNow]:https://ephos.github.io/tag/didyouknow
[about_Foreach]:https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_foreach?view=powershell-7
[keywords]:https://github.com/PoshCode/PowerShellPracticeAndStyle/blob/master/Style-Guide/Code-Layout-and-Formatting.md
[ForEach-Object]:https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/foreach-object?view=powershell-7
[about_Arrays]:https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_arrays?view=powershell-7#foreach