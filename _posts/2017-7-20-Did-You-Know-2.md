---
layout: post
comments: true
title: Did you know?
tags: didyouknow powershell
---

### Did you know - PowerShell Console, "#" Console Searching!

Here's one my colleague recently discovered that has changed my (PowerShell) life.  History is a great thing, it enables us to be lazy and/or save time.  How many times have you been at the console doing some kind of interactive work and said *"Ugh what was the command I used 30 minutes ago that I'd only need to change a few things to reuse..."*?  I know for me it'd happen a lot, especially doing a lot of PowerCLI work.  I had some custom aliases for making Get-History and Invoke-History easier to access but little did I know there was an even easier way.

**Enter the "#"...**

Depending on your age or geographic location you've probably had in depth and long philosophical conversations on if this is called the pound sign, hash, hashtag, number sign, or octothorpe.  Regardless it's an amazing little tool in the PowerShell console.

Say you've been doing some work but needed to recall a command you ran over an hour ago!  You could hit "Up Arrow" in the console to scroll through all of them but you might have run hundreds of commands or not really recall how many ago it was.  You could run `Get-History` followed by copying the command or running `Invoke-History`, but this is at least 2 whole steps and some extra typing.  I need to run the command to list the contents of *C:\Windows\\*.

Use the **#**!

(Showing my history in this example.  We want to run the second item.)
![Alt+Enter]({{ site.baseurl }}/images/2017-7-20-Did-You-Know-2/DidYouKnow2_1.PNG)

You just type the `#` followed by part of the text you want to search for, lets use "windows" as our example and then **tab**.

```powershell
#windows
```

It looks strange, since it seems like you're typing in a comment, but trust me.  You'll find you can tab through all of your history items that match 'windows'.  Pretty cool!