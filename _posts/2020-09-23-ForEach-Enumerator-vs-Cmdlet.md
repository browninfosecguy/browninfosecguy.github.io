---
layout: post
title: "PowerShell ForEach Enumerator vs ForEach-Object Cmdlet "
twitter_image: /images/PowerShell-ForEach/Diagram3.png
description: "The difference between foreach loop and cmdlet."
date: 2020-09-23
feature_image: /images/PowerShell3.jpg
tags: [PowerShell]
hashtags: PowerShell
---
The ForEach enumerator and ForEach-Object can be confusing at times. In this blog post I will highlight some differences between the two.
<!--more-->

The ForEach-Object cmdlet is used to process one object at a time passed through the pipeline. The ForEach-Object cmdlet has three main parameters BEGIN, PROCESS and END. The BEGIN script block run once before anything is processed from the pipeline, the PROCESS script block is the main processing block where objects from the pipeline are processed. The PROCESS block is run once for every object in the pipeline. Finally, the END script block is run at the very end.

{% include diagram.html imageurl="/images/PowerShell-ForEach/Diagram3.png" caption="Figure1: Visual Representation of ForEach-Object" %}

Let’s take a simple example, suppose we want to count all the processes where Name property is "chrome". We begin by running Get-Process cmdlet and passing all the returned objects to the ForEach-Object cmdlet in the pipeline for further processing. 

The ForEach-Object first initializes a variable called $BrownserCount with an initial value of zero in the BEGIN script block. If you remember, the BEGIN script block is only run once before we process anything from the pipeline. Next, the PROCESS script block start processing the objects from the pipeline one by one. We compare the Name property of each object with 'chrome' and increment the $BrowserCount variable by 1 for each match. Finally, the END script block writes the final value of $BrowserCount variable back to pipeline.

```PowerShell
Get-Process | ForEach-Object -Begin {$BrowserCount=0} -Process { if($_.Name -eq 'chrome'){$BrowserCount++} } -End {$BrowserCount}
```


The ForEach enumerator works differently, to begin with we don't have the BEGIN, PROCESS and END block. The ForEach enumerator does not write back to the pipeline. Moreover, we cannot use the $_ variable.

As shown in figure 1 when we try to use ForEach enumerator along with Measure-Object to calculate the average it fails as ForEach enumerator does not write to the pipeline.

{% include diagram.html imageurl="/images/PowerShell-ForEach/Diagram1.png" caption="Figure2: ForEach Enumerator" %}

In order to correctly perform this operation, we use the ForEach-Object cmdlet as shown in figure 2 below. 

{% include diagram.html imageurl="/images/PowerShell-ForEach/Diagram2.png" caption="Figure3: ForEach-Object cmdlet" %}

{% include thanks.html %}




