---
layout: post
comments: true
title: -OutVariable, A Cautionary Tale...
tags: powershell
---

In PowerShell we are provided common parameters.  A common parameter is a parameter that is available on every Cmdlet or also on any advanced function that is decorated with the `[CmdletBinding()]` attribute before the param block.

One of my favorite common parameters *was* -OutVariable.  It allowed me to type a really long command out and then at the end if I decided I wanted to store it in a variable I could just append the -OutVariable or -ov parameter and then the variable name.

~~~ powershell
Invoke-ExampleCommand -Parameter1 Foo -Parameter2 Bar -Parameter3 Fiz -Parameter4 qux -OutVariable variableName
~~~

It saved me from having to bounce back to the front of the command using either the Ctrl+LeftArrow or the Home key.  Yes I am literally that lazy, the fewer keystrokes the better!

~~~ powershell
$variableName = Invoke-ExampleCommand -Parameter1 Foo -Parameter2 Bar -Parameter3 Fiz -Parameter4 qux
~~~

Both commands above accomplish the same thing, but the second one might have required me spending those extra moments getting to the front of the command to store it.

I used -OutVariable for awhile before I learned it had a **dark** secret, it wasn't the friend I knew or first met to save me seconds on storing a variable when doing PowerShell from the shell itself.  It was a wolf.. um, ArrayList in.. err... some other type's clothing.

Let me explain...

I wanted to write a function for some DNS work that accepted a CimSession as a parameter type.  One of my really basic profile functions is a quick wrapper for `New-CimSession` to connect to my internal DNS server and return the CimSession as a variable (*for use with DNS Cmdlets*).  To do this I was using -OutVariable.

To show you what I ran into I set up this little test...

```powershell
#Open a PowerShell CimSession to the DNS server, store it in a variable named 'dnsCimSession'
New-CimSession -ComputerName kappa -Credential $myCredential -OutVariable dnsCimSession

#Test function that accepts a single parameter which needs to be of type CimSession (Microsoft.Management.Infrastructure.CimSession).
#It should just return the CimSession if it runs.
function Test-OutVariable
{
    [CmdletBinding()]
    param
    (
        [Parameter(Mandatory = $true, Position = 0)]
        [CimSession]
        $CimSession
    )
    process
    {
            Write-Output -InputObject $CimSession
    }
}
```

This test function CimSession parameter *should* take our CimSession from our 'dnsCimSession' variable that the New-CimSession returned, right?

![OutVariable1]({{ site.baseurl }}/images/2017-5-29-Out-Variable/OutVariable1.PNG)

What the?...
 Cannot process argument transformation on parameter 'CimSession'. Cannot convert the "System.Collections.ArrayList" value of type "System.Collections.ArrayList" to type "Microsoft.Management.Infrastructure.CimSession".

I passed it a variable that contained a type of CimSession though, who said anything about an ArrayList?

![OutVariable2]({{ site.baseurl }}/images/2017-5-29-Out-Variable/OutVariable2.PNG)
![OutVariable3]({{ site.baseurl }}/images/2017-5-29-Out-Variable/OutVariable3.PNG)

Where does the ArrayList come from?  It looks like a CimSession to me!  The thing is, and quite obviously by now, it's not...  Doing a get type on the variable will reveal this. `$dnsCimSession.GetType()`
![OutVariable4]({{ site.baseurl }}/images/2017-5-29-Out-Variable/OutVariable4.PNG)

It is an ArrayList.  I did some testing by declaring a new ArrayList and adding some Int32 values to it and sure enough PowerShell's Get-Member returns the ArrayLists containing type rather than the type of ArrayList itself.
![OutVariable5]({{ site.baseurl }}/images/2017-5-29-Out-Variable/OutVariable5.PNG)

Why does -OutVariable return an ArrayList though?  Lets check the help!

```powershell
Get-Help -Name about_CommonParameters -Detailed
```

***(Excerpt below from the Get-Help)***

```text
 -OutVariable [+]<variable-name
    Alias: ov

    Stores output objects from the command in the specified variable and
    displays it at the command line.

    To add the output to the variable, instead of replacing any output
    that might already be stored there, type a plus sign (+) before the
    variable name.

    For example, the following command creates the $out variable and
    stores the process object in it:

        Get-Process PowerShell -OutVariable out

    The following command adds the process object to the $out variable:

        Get-Process iexplore -OutVariable +out

    The following command displays the contents of the $out variable:

        $out
```

Would you look at that!?  The -OutVariable common parameter has an ability to append more values to the same variable.  I honestly can't think of a use case for this but it seems like the PowerShell developers made a design decision to give -OutVariable this ability in the beginning.  I did some additional digging and found another person who ran into this that posted on StackoverFlow as well as a Github issues filed for it.

- [StackOverFlow Post](https://stackoverflow.com/questions/40666291/using-outvariable-creates-arraylist)
- [GitHub PowerShell Issue 3154](https://github.com/PowerShell/PowerShell/issues/3154)
- [GitHub PowerShell Issue 3773](https://github.com/PowerShell/PowerShell/issues/3154)

It appears that it has been this way since the beginning.  It was up for committee review and it was decided that the behavior for these common parameters should be to return the type the user expected rather than an ArrayList.  It looks as if it was added to the PowerShell 6.0.0 milestone, exciting times!  In the meantime I've just stopped using -OutVariable, and if you choose to use it, be aware you're getting an ArrayList back rather than the type you think it will be.