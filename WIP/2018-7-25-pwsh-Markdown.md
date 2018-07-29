---
title: PowerShell and Markdown
teaser: Markdown is pretty amazing, but having functionality built into PowerShell to interface with it is even better!
category: powershell
tags: [powershell]
reddit_post:
featured_comments:
---

## PowerShell (Core) and Markdown

Markdown is a great way to document, well anything!  If you haven't had a chance to write it yet a time will probably come sooner than later when you'll have a case to use it.

I personally love markdown, it's not a stretch to say I use it literally every day.  As I type this now, this entire blog is based on Jekyll which converts markdown into HTML so I am literally writing markdown right now.  I use it at work in our Visual Studio Team Service wiki for team documentation.  Lastly probably the most crazy thing I've done is started the process of taking years of my One Note notebooks and converting them to mark down in a Github repository _(which I intend to make public someday soon)_.

Today I was reading the [patch notes][patch] for PowerShell *Core* 6.1.0 Preview 4 and saw some markdown notes that got me really excited!  Under the "General Cmdlet Updates and Fixes" section there is a bullet for "_Add Markdown rendering cmdlets ([#6926][issue])_".  Let's take a look!

### Just Show Me How to Use It

At the time of writing, you will need to have installed PowerShell Core 6.1.0 Preview 4.  If you stumble on this blog in the future and are running a minimum of PowerShell Core 6.1.x this functionality should be available to you.

![pwshVersion]({{ site.baseurl }}/images/2018-7-25-2018-pwsh-Markdown/pwshMarkdown1.png)

Good ol' `Get-Command` should tell you what markdown related commands are available.

```powershell
Get-Command -Noun Markdown*
```

You can see we have 4 commands available for some markdown action.

![commands]({{ site.baseurl }}/images/2018-7-25-2018-pwsh-Markdown/pwshMarkdown2.png)

There are two ways you can use this new markdown functionality in PowerShell.  Using `Show-Markdown` you can take markdown and display it as a VT100 encoded string right in the console.  Alternatively you can use `Show-Markdown` to display a markdown file in a web browser.

What the cuss is VT100?  I am admittedly not an expert, but I do know ANSI/VT100 enabled consoles/terminal emulators are able to display colored text.  Before you continue, go ahead and run `Get-MarkdownOption`.

![vt100]({{ site.baseurl }}/images/2018-7-25-2018-pwsh-Markdown/pwshMarkdown3.png)

You'll see that you have different markdown elements like Header1-6, Code, Link, Image, etc.  Each has an associated ANSI/VT100 escape code which will determine its color and formatting in the PowerShell console.  These codes look funny and you can see more about them [here][ANSIVT100].  A super quick info on them

[patch]:https://github.com/PowerShell/PowerShell/releases/tag/v6.1.0-preview.4
[issue]:https://github.com/PowerShell/PowerShell/pull/6926
[ANSIVT100]:https://misc.flogisoft.com/bash/tip_colors_and_formatting