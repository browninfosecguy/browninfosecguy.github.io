---
layout: post
title: "PowerShell Operators"
twitter_image: /images/PowerShell-Operators/Diagram7.png
description: "PowerShell Tutorial on operators."
date: 2020-09-30
feature_image: /images/PowerShell-Operators/Diagram7.png
tags: [PowerShell]
hashtags: PowerShell
---
PowerShell includes number of operators which can be used to perform comparisons, string manipulation, matching etc. These operators are categorized into different categories:

1. Comparison Operators
2. Logical Operator
3. Arithmetic Operator
4. Redirection Operator
5. Split and Join Operator
6. Unary Operator
7. Type Operator
8. Special Operator

In this blog post we will look at some "interesting" operators.
<!--more-->

### Subexpression Operator

The subexpression operator evaluates the result and returns a scaler value or an array. The subexpression operator is mainly used when we use an expression within another expression. For example: 

```
Write-Host "There are $((Get-Process -Name 'chrome').count) instances of chrome application running"
```

### Array subexpression operator @( )

The array subexpression operator is similar to the subexpression operator with a difference that results are returned as an array. Moreover, you can't use the array subexpression within another expression. For example, the above example would not work if we replace "$" by "@", this can be observed in figure below

![alt text](/images/PowerShell-Operators/Diagram1.png "@ and $ subexpression operator")

### Call Operator &

The & call operator can be used to execute code or script. For example

```
$myCommand = "Get-Process"
& $myCommand
```
![alt text](/images/PowerShell-Operators/Diagram2.png "Call operator")

However, the above command will fail if we try to supply Get-Process with parameters. For example, "Get-Process -Name 'brave'" will result in an error. The solution is to enclose our command inside a script block as shown below.

```
$myCommand = {Get-Process -name 'brave'}
& $myCommand
```
![alt text](/images/PowerShell-Operators/Diagram3.png "Call operator")

### Format Operator -f

The format operator can be handy when trying to formulate a string. Using format operator, we can formulate the values on the left side using the objects on the right side of the format operator. It might sound familiar along the lines of using function with parameter.

![alt text](/images/PowerShell-Operators/Diagram4.png "Format operator")

### Type Operator

The type operators (-is, -isnot, -as) are used to evaluate the type of .NET object. For example:

```
$variable = "Happy Days"

$variable -is [String]
```
![alt text](/images/PowerShell-Operators/Diagram5.png "Type operator")

It returns True if the object type matches else false is returned. 

The "-as" operator works differently, it is typically used to generate a new object with data type on the right side of the "-as" operator. If the operation cannot be performed, then $null object is returned. For example, below we are using the "-as" operator to generate a new object of type DateTime for Date comparison. 

![alt text](/images/PowerShell-Operators/Diagram7.png "As operator")

### Match Operator

The Match operator comes in two flavors "-match" and "-nomatch". The match operator is based on regular expressions. As shown in the examples below the match operator uses regular expression for comparing two objects.

```
PS C:\> $statement ="Gin and Soda is perfect for the Weekend"
PS C:\> $statement -match "^Gin"
True
PS C:\> $statement -match "^Soda"
False
PS C:\> $statement -match "Weekend$"
True
PS C:\> $statement -match "Perfect$"
False
PS C:\>               
```
### Like Operator

The like operator uses wildcard characters for comparison. For example:

```
PS C:\> $statement ="Gin and Soda is perfect for the Weekend"
PS C:\>
PS C:\> $statement -like "*Soda"
False
PS C:\> $statement -like "*Soda*"
True
PS C:\> $statement -like "Gin?*Soda*"
True
PS C:\> $statement -like "and?*Soda*"
False
PS C:\> $statement -like "*and?*Soda*"
True
PS C:\> $statement -like "*nd?*Soda*"
True
```

### Containment Operator

The containment operator (-in, -notin, -contains, -notcontains) is used to determine if a value exist in an array or not. For example:

```
PS C:\> $variable = "brady","bill","josh","slater"
PS C:\>
PS C:\> "brady" -in $variable
True
PS C:\> $variable -contains "brady"
True
PS C:\> $variable -contains "cam"
False
PS C:\>      
```
{% include thanks.html %}
