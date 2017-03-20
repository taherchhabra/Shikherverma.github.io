---
published: true
layout: post
subtitle: javascript global variabe
date: 2011-09-15T12:23:33.000Z
author: Taher Chhabra
header-img: img/posts/Simple-Blue-Background.jpg
comments: true
tags:
  - Design Pattern
---
I was reading about JQuery and came to know something about Javascript global variables.
A Javascript glabal variable is declared as follows

var myGlobalVar = 5;
function myFunction()
   alert(myGlobalVar); // 5
}

The above code will give you result 5
Now use the code given below

var myGlobalVar = 5;
function myFunction()
   alert(window.myGlobalVar); // 5
}
 
Voila !!
The Global variable became the property of the window object. 
 