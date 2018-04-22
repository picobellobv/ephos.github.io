---
title: Where to Begin?
teaser: You want to start learning PowerShell, but where do you even start?! Hint; Here!
category: powershell
tags: [learning, powershell]
---

The age old question... *"Where do I start?"*

I started learning PowerShell in 2013 mostly by my own stubbornness and loathing of manual tasks.  I had a task where I needed to make sure a setting was set on hundreds of servers, and knew that doing it by hand would take me multiple days of clicking and typing through screens.  It was at that moment I first opened the PowerShell ISE and struggled for about a whole day and a half furiously writing, Googling, and testing the most basic and crude plain old .ps1 file I ever wrote.  I remember the first time I ran it when it was finished, I watched as this thing **I** built turned literal days of work into minutes.  I felt so accomplished that I had created something to do work for me.  From that day on every task I needed to do I approached with a completely new mindset.

Realistically however, the time saved was probably minimal as it took me quite awhile to write it.  This is a fact you need to accept however when learning a new language.  The knowledge you will gain however is worth this investment, and as you progress the up-front time to accomplish certain tasks will decrease.

Luckily for newcomers there are some awesome **free** resources you can start to check out!

In my opinion the absolute best place to start is the *Getting Started with PowerShell* Microsoft Virtual Academy video with Jason Helmick and PowerShell creator Jeffrey Snover.

**[Getting Started with PowerShell](https://mva.microsoft.com/en-us/training-courses/getting-started-with-powershell-3-0-jump-start-8276?l=r54IrOWy_2304984382)**

My second suggested resource(s) are the free eBooks on GitBook provided by the DevOps Collective.  To this day I still reference The Big Book of PowerShell Gotchas for clarity on occasion on some topics.
**[The DevOps Collective eBooks](https://www.gitbook.com/@devopscollective)**  *(The **[PowerShell.org YouTube](https://www.youtube.com/channel/UCqIw7UUwC5fUBFXYX68aMrQ)** page has been converting these to videos as well.)*

In regards to how your code should look or be styled there is no better free place for this than PoshCode's PowerShellPracticeAndStyle repository.  In short what this refers to is how readable your code is and that you are following a basic guideline of rules in regards to writing code *(i.e. always declaring the [CmdletBinding()] attribute on a function)*.
**[PowerShellPracticeAndStyle](https://github.com/PoshCode/PowerShellPracticeAndStyle/blob/master/Style%20Guide/Introduction.md)**

As for built-in resources, hit up the about help files!  Make sure you run `Update-Help` first if you haven't yet to get the most updated help files.
![Get-Help]({{ site.baseurl }}/images/2017-2-14-Where-to-Begin/WhereToBegin1.PNG)

Lastly, the community!  Never hesitate to ask a question in fear that you think it might be dumb.  If you do wind up asking a question, just be sure that you include as much information possible on the question, where you are stuck, and what you've tried so far.  Don't expect anyone to write code for you, even if you are able to convince someone to, or they do it out of the kindness of their heart you'll only have cheated yourself by not continuing to learn something new.

* **[/r/PowerShell Subreddit](https://www.reddit.com/r/PowerShell/)**
* **[PowerShell.org Forums](https://powershell.org/forums/)**

Don't forget to check out your closest PowerShell user group as well.

That is all, there are plenty of other great resources out there as well but I wanted to focus on the free ones anyone can access as cost shouldn't be a barrier to begin learning PowerShell.  Learning something new can sometimes feel daunting.  There may be times you get stuck for hours, maybe even days.  There will be other times you feel like you are constantly wrestling topics that you just can't seem to get figured out.  At rare times you may even feel like you've regressed.  I assure you if you stick with learning PowerShell, you will come to find you'll see the payoff as time goes on.  You'll likely want to start automating all of the mundane tasks that eat up so much time.

Update 4-22-2018[^1]

---

[^1]:
    I went through a big update in April 2018, this article was moved as part of that!