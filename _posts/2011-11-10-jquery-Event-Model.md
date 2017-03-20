---
published: true
layout: post
subtitle: Jquery Event Models
date: 2011-11-10T12:23:33.000Z
author: Taher Chhabra
header-img: img/posts/red-background.jpg
comments: true
tags:
  - jquery
---
We all know the difference between a Standards Complaint browser and Internet Explorer. Those quirks are in the Event handling mechanism as well.

This all started when Netscape corporation introduced an event handling model in their Netscape Navigator browser, It was called Netscape Event Model or Basic Event Model but majority of Web Developers know it as DOM Level 0 Event Model.

Then in November 2000 W3C introduced a standardized model for Event handling which is known as DOM level 2 Event Model. All modern browsers support this model except Internet Explorer (As usual) ,IE uses its own proprietary Model.

So lets have a look at these two models before we see how JQuery makes life easier.

### DOM Level 0 Event model
In this event model the event handlers are assigned by assigning the reference of the event handler to the corresponding properties of the element

    <html>
    <head> 
    <title>Dom level 0 event model</title>
    <script type="text/javascript" src="jquery-1.4.js"></script>
    <script type="text/javascript">
    $(function(){ 
         $('#example')[0].onmouseover = function(event) { 
                                             alert('on mouse over');
                                        };
     }); 
    </script>
    </head>
    <body>
    <img id="example" src="test.jpg" height="500px" width="500px">
    </body>
    </html>

In the above code we are assigning the onmouse event handler code to the onmouseover property of the image element

### DOM Level 2 Event model

The disadvantage of DOM Level 0 event model was that, only one event handler can be attached for an event. Since the event handler are referenced by the properties only one handler can be አስስግነድ

In the above code if we write these two statements

$('#example')[0].onmouseover = function(event) { alert('first mouse over'); };

$('#example')[0].onmouseover = function(event) { alert('second mouse over'); }; 


The first event handler will be replaced with the second event handler. So in DOM Level 2 Event Model there is a method addEventListener(eventType,listener,useCapture).

    <html>
    <head>
    <title>Dom level 0 event model</title>
    <script type="text/javascript" src="jquery-1.4.js"></script>
    <script type="text/javascript">
    $(function(){ 
         var element = $('#example')[0]; 
         element.addEventListener('click',function(event) { alert('BOOM !'); },false); }); 
    </script> 
    </head> 
    <body> 
    <img id="example" src="test.jpg" height="500px" width="500px"> 
    </body>
    </html>


### Internet Explorer Event Model

Instead of addEventListener method, Internet Explorer event Model uses attachEvent method having the following signature attachEvent(eventName,handler)

### JQuery Event Model

Jquery's event model provides a unified api for different browsers. It allows multiple handler for a event and makes Event instance available as parameter to the handlers

In JQuery, we can establish event handlers on DOM elements using the bind method.

bind(eventType,data,handler)

    <html>
    <head>
    <title>Dom level 0 event model</title>
    <script type="text/javascript" src="jquery-1.4.js"></script>
    <script type="text/javascript"> 
    $(function(){ 
    var element = $('#example')[0];
    element.bind('click',function(event) {alert('on click!');}) }); 
    </script>
    </head>
    <body>
    <img id="example" src="test.jpg" height="500px" width="500px">
    </body>
    </html>

you can chain multiple events as shown below

    <html>
    <head>
    <title>Dom level 0 event model</title>
    <script type="text/javascript" src="jquery-1.4.js"></script>
    <script type="text/javascript">
     $(function(){
     var element = $('#example')[0]; 
    element.bind('click',function(event) {alert('Boom !');}).bind('click',function(event) {alert('Zoom!');}) });
    </script> 
    </head>
    <body>
    <img id="example" src="test.jpg" height="500px" width="500px">
    </body>
    </html>


The Event handlers can be removed using unbind method

unbind(eventType,listener)