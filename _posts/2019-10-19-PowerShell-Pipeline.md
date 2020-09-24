---
layout: post
title: "PowerShell Pipeline "
twitter_image: /images/PowerShell-Pipeline/Diagram5.png
description: "The PowerShell Pipeline."
date: 2019-10-19
feature_image: /images/PowerShell3.jpg
tags: [PowerShell]
---
Pipeline in PowerShell is an easy yet very important concept to understand. Before diving into pipeline for PowerShell it’s important to understand pipelines in general. The concept of pipelines originates from Unix world. In simple terms pipeline is a thread to stich together different comands to produce desired output.
<!--more-->
Below is a simple command showing how pipeline can be used to produce desired result.

{% include diagram.html imageurl="/images/PowerShell-Pipeline/Diagram1.png" caption="Figure1: Simple command using Pipe in bash" %}

As we can see we are able to filter results from ps command for process named chrome. We further narrowed down the result to just display the first line by using “head -1” command.
We can create more complex filters by using regular expressions. In *nix based system an output from one command is passed on as text to the next command in the pipeline. This is shown below

{% include diagram.html imageurl="/images/PowerShell-Pipeline/Diagram2.png" caption="Figure2: Same command in last example but in PowerShell" %}

It produces same result but we are manipulating objects not text. Get-Process cmdlet get a list of process objects, passes it on to Where-object cmdlet which filters the results to just keep objects whose “Name” Property matches the value chrome. Next we just select the first object and discard the rest.
In case you don’t trust me we can use the Get-Member cmdlet to view Object type, different properties and methods.

{% include diagram.html imageurl="/images/PowerShell-Pipeline/Diagram3.png" caption="Figure3: Object Members which include properties and methods" %}

If you are unfamiliar with Objects I would recommend reading Object oriented concepts. Here is a nice and quick read.
[Understanding Classes and Objects](https://www.ncl.ucar.edu/Document/HLUs/User_Guide/classes/classoview.shtml)

But at high level you can think about object as a person. A person will have properties such as black hair, brown eyes etc. and some methods, in laymen terms “things” this person can do for example dance, wash dishes etc. So this person is an object of Type “Human Being” and has member properties (black hair, brown eyes) and member functions (can dance, can cook).

{% include diagram.html imageurl="/images/PowerShell-Pipeline/Diagram4.png" caption="Figure4: In this example the person is an object with properties and methods" %}

The diagram below shows PowerShell pipeline in action.

{% include diagram.html imageurl="/images/PowerShell-Pipeline/Diagram5.png" caption="Figure5: PowerShell Pipeline" %}

It might look simple but there is lot going on under the hood. PowerShell works hard to determine how the next cmdlet in the pipeline consumes objects generated from the last cmdlet. To understand that we need to understand Parameter binding in PowerShell.
Let’s take an example, even though it would be disastrous idea, let’s say that you want to stop all processes on a system. How would you do it? well you will run something like “Get-Process | Stop-Process”.
In the command above Get-Process generate process objects which reflect all the running processes on a system. Next this collection of process objects is passed on to the next cmdlet in the pipeline, in our case it’s Stop-process.
At this point PowerShell need to determine how to pass these objects to Stop-process cmdlet. This is called Parameter binding.
Parameter binding is an important concept to fully understand PowerShell pipeline. So let’s go back to our example, at this point PowerShell need to determine how to pass the objects to next cmdlet, at this point PowerShell checks the next cmdlet to determine if it can do Parameter binding byValue or byProperty.
In order to make that decesion PowerShell first check to see if the next cmdlet accept parameter byValue. In our example, lets check that manually

{% include diagram.html imageurl="/images/PowerShell-Pipeline/Diagram6.png" caption="Figure6: Parameter Binding byValue" %}

As you can see Stop-Process has a parameter set which accept an InputObject ByValue.
byValue means that the cmdlet can consume the incoming object as “Whole” meaning we can pass on the object as a parameter and the cmdlet will work.
If you are lost stay with me because once you see byProperty things will become clear.
But visually here is what this process looks like

{% include diagram.html imageurl="/images/PowerShell-Pipeline/Diagram7.png" caption="Figure7: Parameter Binding " %}

You can inspect the parameter binding byValue by using the Trace-Command. Here is what i used to run my trace command thanks to Dr Scripto

{% include diagram.html imageurl="/images/PowerShell-Pipeline/Diagram8.png" caption="Figure8: Dr Scripto " %}

In the output below you can see the binding happening byValue

{% include diagram.html imageurl="/images/PowerShell-Pipeline/Diagram9.png" caption="Figure9: Trace Cmdlet" %}

If you wan’t to learn about Trace-Command [here](https://devblogs.microsoft.com/scripting/trace-your-commands-by-using-trace-command/) is a good resource.

Now let’s “assume” that byValue is not option, then the next option is to do parameter binding byProperty, so PowerShell determines which properties of the incoming object can be used as parameters for the next cmdlet in the pipeline. Example if incoming object has a property called “Name” the PowerShell parameter binding will insert the value of “Name” property into the “Name” parameter of the cmdlet.
Let’s take an example to solidify this concept. Lets say I want to get proceses for Edge browser. Here is a simple one liner.

{% include diagram.html imageurl="/images/PowerShell-Pipeline/Diagram10.png" caption="Figure10: Get-Process cmdlet" %}

Pretty easy, right? What if, I want to kill the Edge process well lets try to do it.

{% include diagram.html imageurl="/images/PowerShell-Pipeline/Diagram11.png" caption="Figure11: Pipeline error" %}

Well that didn’t work; Why didn’t PowerShell bind the Name property automatically to the “Name” parameter of Stop-process cmdlet?
Because“MicrosoftEdge” is an object of String type, and this object does not have a Property called “Name” hence both Parameter Binding techniques, byValue and byProperty failed.
In order to make this work we need to create a custom object and add the Property called “Name” to it and then pass this object to stop-process as shown below.

{% include diagram.html imageurl="/images/PowerShell-Pipeline/Diagram12.png" caption="Figure12: We are creating a custom PSObject with Property 'Name' " %}

In the figure above we created a new object of type PSObject then added a property named “Name” with value “MicrosoftEdge”. Lastly, we passed this object to Stop-Process cmdlet. Below you can see parameter binding using Trace-command cmdlet.

{% include diagram.html imageurl="/images/PowerShell-Pipeline/Diagram13.png" caption="Figure13: Parameter binding byProperty" %}

I hope this writeup helped you if you were struggling with PowerShell Pipeline binding concepts.

{% include thanks.html %}

**External References:**

+ https://sites.google.com/site/tmylearning/psparam

+ https://www.ncl.ucar.edu/Document/HLUs/User_Guide/classes/classoview.shtml

+ https://devblogs.microsoft.com/scripting/trace-your-commands-by-using-trace-command/

+ https://www.manning.com/books/learn-windows-powershell-in-a-month-of-lunches?query=PowerShell

