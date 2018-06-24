---
title: Objects, PowerShell, and You!
teaser: When PowerShell doesn't include the tools you need, sometimes the ol' .Net framework can fill those gaps for you!
category: powershell
tags: [powershell, .net]
reddit_post:
featured_comments:
---

## Objects, PowerShell, and You!

### Overview

Today you're going to learn about utilizing parts of .Net with PowerShell and some of the associated quirks.

### Level Set

If you've been using PowerShell Core or Windows PowerShell for any length of time you likely know that under the hood it's written in C# and utilizing the [.Net Framework](dotnet), .Net Core for PowerShell Core and the Windows only .Net Framework for Windows PowerShell.  If you didn't know that, well, now you do!

PowerShell Cmdlets are typically written in C# and compiled, though if you've been nosey enough to poke around some of the other modules shipped with PowerShell you'll find some common commands are just functions (usually advanced functions) written in PowerShell itself, like the [PowerShellGet](PowerShellGet) module.

Okay you're thinking, _"I already have all the commands I need with the cmdlets and functions that exist"_.  Some of you may not have run into this yet however, a time will come where you'll need to dip into the .Net framework to get what you need.  Don't fear the reaper... whoops, no that's Blue Oyster Cult, yeah, don't fear the framework!

This might get a little developer-y than you might be used to, good!  Dig in and keep reading!

### A World of GUIDs

To start simple, PowerShell elders likely remember a time when PowerShell couldn't generate a [GUID](GUID) with a command.  A GUID is a 128-bit globally unique identifier, typically they are used to give something a super unique identification number, examples may be that you need to give a temporary object/server/anything an identification number, and sequential numbers wouldn't make as much sense, as the object may be unchanged _(immutable)_ and just deleted and recreated rather than modified.

Also, if you've done any PowerShell Desired State Configuration work, you've likely run into a GUID at one point or another.

#### Example 1 - PowerShell Cmdlet

PowerShell 5.x introduced the `New-Guid` Cmdlet in the Microsoft.PowerShell.Utility module that comes with PowerShell.  If you've used it, you've noticed that the usage is... super trivial.

```powershell
New-Guid
```

_Console Output Below_

```text
PS> New-Guid

Guid
----
3c801aed-253d-430d-8ddb-05c66e7e4358
```

#### Example 2 - Type Accelerator

If you had to create a GUID in PowerShell prior to this, you would have had to access .Net namespaces and classes directly with PowerShell.

```powershell
# Note: PowerShell automatically prepends the 'System' Namespace, there is no harm in explicitly adding it however.
[System.Guid]::NewGuid()
```

_Console Output Below_

```text
PS> [System.Guid]::NewGuid()

Guid
----
9a63516e-bef2-44df-8e46-1af60df8dcee
```

You'll notice the syntax isn't as much like PowerShell as you may have seen before.  This syntax using what is called a type accelerator.  This is indicated by the square brackets `[` `]`.  The double colons (`::`) is called a [static member][staticmember] operator.  This type accelerators syntax allows you to call a class and one of its static methods or properties directly.  In our example we used the `NewGuid()` method, and because it is a method which doesn't require any inputs we need to declare its open and close parenthesis with nothing in them.

Depending on your background this might look more natural, or super strange.  Developers coming from a C#, F#, Visual Basic world may prefer the second way, where PowerShell developers might prefer the first way.

#### Example 3 - New-Object

You may be thinking,  _"In the past I have used the New-Object Cmdlet..."_.  You are correct, this however is where one of the quirks lies.  Take a look at GUIDs again here.

This will generate an empty GUID of all 0's.

```powershell
New-Object -TypeName System.Guid
```

This is happening because the class generate a GUID (System.Guid) has a default constructor which, when it is instantiated (via PowerShell or any other .Net based language), will create an empty GUID.

This is because System.GUID is also a value type (because it is a structure/struct) and not a reference type.  When a value type is instantiated and uses its default constructor, it will get a default result.  Some additional examples can be seen below.

```powershell
New-Object -TypeName System.Int32 #Will result in "0"
New-Object -TypeName System.Boolean #Will result in "False"
New-Object -TypeName System.DateTime #Will result in "Monday, January 1, 0001 12:00:00 AM"
```

You will find this quirk when using `New-Object` as it will create the object, just not in the most ideal fashion.

### Terminology

"Reference types?  Value types?  Instantiation?  Namespaces?  Structures?  What does this all mean?"

If you are coming from an operations background and worked your way into PowerShell from just trial and error some of these terms may be new to you.  Before continuing you should get familiar with these terms.  There will also be some we didn't cover yet that will come up later.  These meanings are not Merriam Webster definitions but hopefully in more layman's terms to the average user.

| --- | --- |
| Term | Meaning |
| Class / Struct | A blueprint used to create a new object when instantiated / created. |
| Type | The type of a created object, regardless of it was created from a class or a struct, it will have a type.  `Get-Member` will show its base type. |
| Namespace | |
| Instantiate | |
| Constructor | |
| Reference Type ||
| Value Type  ||

[^1]:
    Place Hold

[GUID]: https://en.wikipedia.org/wiki/Universally_unique_identifier
[PowerShellGet]: https://github.com/PowerShell/PowerShellGet
[dotnet]: https://www.microsoft.com/net/download/windows
[staticmember]: https://docs.microsoft.com/en-us/powershell/scripting/getting-started/cookbooks/using-static-classes-and-methods?view=powershell-6