---
layout: post
title: "PowerShell Out-GridView cmdlet with PassThru Switch"
twitter_image: /images/PowerShell-OutGridView/Diagram3.png
description: "A neat use case of using Out-GridView with PassThru."
date: 2020-09-25
feature_image: /images/PowerShell-OutGridView/Diagram3.png
tags: [PowerShell]
hashtags: PowerShell
---
I recently learned this near use case of using Out-GridView cmdlet with PassThru switch. This blog post I will be discussing how we can use Out-GridView in the Pipeline to present user a graphical way to select certain objects for further processing down the pipeline.
<!--more-->

PowerShell pipeline is very effective in processing objects as they arrive in the Pipeline. Most of the time a user with stich together cmdlets and let pipeline do its magic. However, sometimes we might want a way to graphically view the objects which are going to be passed along the pipeline or better select certain object in a visual manner before passing them along the pipeline for processing.

I learned this "trick" while watching [Jeff Hicks](https://twitter.com/JeffHicks) videos on [Pluralsight](https://pluralsight.com).

## PassThru Switch Parameter
The PassThru switch parameter is used to force a cmdlet to return an object back to the pipeline. PowerShell has number of cmdlets whose default behavior is set to process the input objects but not generate any objects as output. If the cmdlet supports "PassThru" switch parameter, then we can force the cmdlet to generate output objects.

You can view the list of cmdlets which support PassThru switch parameter by running following PowerShell command
```
Get-Command -ParameterName PassThru
```
On my Windows 10 machine it generates following:

{% include diagram.html imageurl="/images/PowerShell-OutGridView/Diagram1.png" caption="Figure1: Cmdlets supporting PassThru Switch" %}

## Out-GridView Cmdlet

The Out-GridView cmdlet send the output to a grid view window. The output is presented to the user in an interactive tabular format with the option to sort and filter the results as shown in figure below.

{% include diagram.html imageurl="/images/PowerShell-OutGridView/Diagram2.png" caption="Figure2: Interactive Grid View with filtering option" %}

## Use Case

Now let's take a use case where I want to present user with a list of Processes running on the system and let the user select some processes to terminate. We can combine the Out-GridView cmdlet with PassThru parameter to accomplish this.
Our PowerShell code would look something like shown below; 

```
Get-Process | Out-GridView -PassThru | Stop-Process
```
We are presented with the grid view window as shown in figure below

{% include diagram.html imageurl="/images/PowerShell-OutGridView/Diagram3.png" caption="Figure3: Grid View window with list of Processes" %}

For demonstration purposes I'm using Stop-Process with WhatIf switch, but we can clearly see that after selecting "Brave" process it was passes on to the next cmdlet in the Pipeline for processing. In our example the Stop-Process cmdlet would have terminated our brave process.

{% include diagram.html imageurl="/images/PowerShell-OutGridView/Diagram4.png" caption="Figure4: Stop-Process terminating the selected processes" %}

We can also use Out-GridView to visually prompt user to select single or multiple values for a variable. For example, the following code will prompt user for selecting a single value and store it to variable 'choice'

```
$choice = 'A','B','C' | Out-GridView -Title "Select one value" -OutputMode Single
```
{% include diagram.html imageurl="/images/PowerShell-OutGridView/Diagram5.png" caption="Figure5: Out-GridView to select value for a variable" %}

We can modify our code to even select multiple values as shown below

```
$process = Get-Process | select Name | Out-GridView -Title "Select processes" -OutputMode Multiple
```

{% include thanks.html %}