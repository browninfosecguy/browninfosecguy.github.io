---
layout: post
title: "PowerShell and Data Analysis"
twitter_image: /images/PowerShell-DataAnalysis/Diagram7.png
description: "PowerShell and Data Analysis"
date: 2019-11-04
feature_image: /images/PowerShell-DataAnalysis/Diagram7.png
tags: [PowerShell]
hashtags: PowerShell,Shodan,JSON
---
This blog post was inspired from a talk me and my buddy [Grant](https://twitter.com/gzboudreau) did at [AtlSecCon](https://twitter.com/AtlSecCon). We analyzed Shodan data for four Atlantic provinces and presented our findings.
For my research and data analysis I leveraged PowerShell and this blog post will present some lessons I learned.
<!--more-->
Shodan give subscribed user an option to download data in different format for offline analysis. For our purposes we decided to download data using JSON format. If you don’t know JSON (trust me it’s more than just tag and value) I would highly recommend following [article](https://www.elated.com/json-basics/) to get the basics right.

PowerShell provides two Cmdlet for JSON, ConvertTo-JSON and ConvertFrom-JSON. As you can imagine ConvertTo-JSON convert the Objects into JSON format while ConvertFrom-JSON convert JSON to PSObject (fancy way of saying PowerShell object). Here is official description of this cmdlet from Microsoft.

*“The ConvertFrom-Json cmdlet converts a JavaScript Object Notation (JSON) formatted string to a custom PSCustomObject object that has a property for each field in the JSON string. JSON is commonly used by web sites to provide a textual representation of objects.”*

But what does this mean?
Lets see an example, here is a small JSON file representing a single object, i saved this file as “shodan.json”

```JSON
{
“source_ip”: “192.168.0.1”,
“port”: 22,
“org”: “Acme”
}
```
{% include diagram.html imageurl="/images/PowerShell-DataAnalysis/Diagram1.png" caption="Figure1: The ConvertFrom-JSON cmdlet was successful" %}

Nothing fancy here right, well under the hood the txt file is converted to objects and we love objects. The screen capture below shows that we converted the txt file to PSCustomObject. (It’s a JSON file but in reality it’s just a txt file with bunch of data following JSON convention)

{% include diagram.html imageurl="/images/PowerShell-DataAnalysis/Diagram2.png" caption="Figure2: The Get-Member clearly reflecting our JSON data converted to Object" %}

Lets take another example this time we have two objects

```JSON
[
{
“source_ip”: “192.168.0.1”,
“port”: 22,
“org”: “Acme”
},
{
“source_ip”: “192.168.0.2”,
“port”: 23,
“org”: “NASA”
}
]
```

Here is the PowerShell cmdlet to convert JSON to PowerShell objects

{% include diagram.html imageurl="/images/PowerShell-DataAnalysis/Diagram3.png" caption="" %}

But why are we converting the JSON file to objects, well once we have the JSON elements as objects we can leverage number of PowerShell cmdlets such as Measure-Object, Sort-Object to accomplish “interesting” things.
Moreover you can then use tool like Gnuplot or good old Excel to create nice looking graphs.
For our example, I wanted to analyze Shodan data containing services listening on Port 21 in the beautiful province of New Brunswick, Canada. So I used my subscription to pull the data from Shodan for Port 21 filtered on New Brunswick and saved it in JSON format. Once I have my JSON file I can start my analysis, to begin here is what the last element in my JSON file

{% include diagram.html imageurl="/images/PowerShell-DataAnalysis/Diagram4.png" caption="" %}

In total we have 1000 record in the JSON file (That’s the default limit Shodan imposes you can increase that)

{% include diagram.html imageurl="/images/PowerShell-DataAnalysis/Diagram5.png" caption="" %}

we can use certain properties and conduct “interesting” analysis. Let me show you how.
Let say I want to see which organization in New Brunswick get’s the Number 1 Rank in exposing insecure port 21 to the internet :) , Here’s how I would do it.

{% include diagram.html imageurl="/images/PowerShell-DataAnalysis/Diagram6.png" caption="" %}

Congratulations GlobalTech Communications you are Ranked Number 1 in exposing Port 21.
Similarly I can do further analysis such as type of software running etc. While PowerShell is not a data analysis platform it sure can help analyze data. I hope this small article help you to see PowerShell potential.

{% include thanks.html %}