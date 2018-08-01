---
title: PowerShell and Markdown
teaser: If you somehow made it to 2018 without using markdown yet, prepare to change that.  Markdown support comes to PowerShell!
category: powershell
tags: [powershell, console]
reddit_post:
featured_comments:
---

## PowerShell (Core) and Markdown

Markdown is a great way to document, well anything!  If you haven't had a chance to write it yet a time will probably come sooner than later when you'll have a case to use it.

I personally love markdown, it's not a stretch to say I use it literally every day.  As I type this now, this entire blog is based on Jekyll which converts markdown into HTML so I am literally writing markdown right now.  I use it at work in our Visual Studio Team Service wiki for team documentation.  Lastly probably the most crazy thing I've done is started the process of taking years of my One Note notebooks and converting them to mark down in a Github repository _(which I intend to make public someday soon)_.

When I was reading the [patch notes][patch] for PowerShell *Core* 6.1.0 Preview 4 and saw some markdown notes that got me really excited!  Under the "General Cmdlet Updates and Fixes" section there is a bullet for "_Add Markdown rendering Cmdlets ([#6926][issue])_".  Let's take a look!

### Just Show Me How to Use It

At the time of writing, you will need to have installed PowerShell Core 6.1.0 Preview 4.  If you stumble on this blog in the future and are running a minimum of PowerShell Core 6.1.x this functionality should be available to you though it's possible the syntax could have changed.

![pwshVersion]({{ site.baseurl }}/images/2018-8-1-pwsh-Markdown/pwshMarkdown1.png)

Good ol' `Get-Command` should tell you what markdown related commands are available.

```powershell
Get-Command -Noun Markdown*
```

You can see we have 4 commands available for some markdown action.

![commands]({{ site.baseurl }}/images/2018-8-1-pwsh-Markdown/pwshMarkdown2.png)

There are two ways you can use this new markdown functionality in PowerShell.  Using `Show-Markdown` you can take markdown and display it as a VT100 encoded string right in the console.  Alternatively you can use `Show-Markdown` to display a markdown file in a web browser.

### Viewing the Markdown

#### In the Console

To parse and view markdown in the comfort of your PowerShell console you can run the following commands, to specify a markdown file, and then display it in the console.  Using this blog post as an example your commands would look like the following.

```powershell
ConvertFrom-Markdown -Path '.\_posts\2018-8-1-pwsh-Markdown.md' -AsVT100EncodedString | Show-Markdown
```

When you run this it will yield results similar to the gif below[^1].

![console]({{ site.baseurl }}/images/2018-8-1-pwsh-Markdown/pwshMarkdown5.gif)

Pretty cool, you can take raw markdown, and view it in a slightly less jarring way in your PowerShell console.  This could be useful when there is a time when you need to work on a system without a Desktop environment, or just for console die hards who want to do as much in a console as possible.

#### In the Browser

You alternatively can view the parsed markdown in the browser.

```powershell
ConvertFrom-Markdown -Path '.\_posts\2018-8-1-pwsh-Markdown.md' | Show-Markdown -UseBrowser
```

![browser]({{ site.baseurl }}/images/2018-8-1-pwsh-Markdown/pwshMarkdown6.png)

Not the prettiest markdown rendering, but it gets the job done.

### Changing the Console Colors

What the cuss is VT100?  I am admittedly not an expert, like, at all.  I do know ANSI/VT100 enabled consoles/terminal emulators are able to display colored text.  To see this, go ahead and run `Get-MarkdownOption`.

![vt100]({{ site.baseurl }}/images/2018-8-1-pwsh-Markdown/pwshMarkdown3.png)

You'll see that you have different markdown elements like Header1-6, Code, Link, Image, etc.  Each has an associated ANSI/VT100 escape code which will determine its color and formatting in the PowerShell console.  These codes look funny and you can see more about them [here][ANSIVT100] if you're interested.

So, now you know how to see what the console colors will be by default, and have a nice link to a reference sheet of what to change them to if you want.  Let's say you want to change 'Header2' so that it is something other than yellow.  You can do that by using `Set-MarkdownOption`.  To make it underlined green you can use `[4;32m`

```powershell
Set-MarkdownOption -Header2Color '[4;32m'
```

You can preview your changes again with `Get-MarkdownOption`

![pwshVersion]({{ site.baseurl }}/images/2018-8-1-pwsh-Markdown/pwshMarkdown4.png)

### Conclusion


[^1]:
    Some say this feels awkward, do I?  Heck yeah, the fact that when using the console you supply the `-AsVT100EncodedString` in the `ConvertFrom-Markdown` command but to view it in the browser you leave this parameter off and instead pass `-UseBrowser` to the `Show-Markdown` command.  It just feels... odd.

[patch]:https://github.com/PowerShell/PowerShell/releases/tag/v6.1.0-preview.4
[issue]:https://github.com/PowerShell/PowerShell/pull/6926
[ANSIVT100]:https://misc.flogisoft.com/bash/tip_colors_and_formatting