---
published: true
layout: post
subtitle: system threading timer wierd behaviour
date: 2012-03-12T12:23:33.000Z
author: Taher Chhabra
header-img: img/posts/red-background.jpg
comments: true
tags:
  - timer
---
Today, I was working on program where in it had to send a string message over a socket after a particular interval. So i thought of using Timer. After searching on the internet i found there are two timers in. Net. System.Timers.Timer and System.Threading.timer.
The difference between the two is that system.threading.timer executes the callback method on a different thread.
I went with system.threading.timer as it suited my requirement. But after some testing i found that the timer callback executed 3 to 4 times and suddenly stopped.
After looking here and there and reading documentation on msdn i found that the timer is garbage collected as soon as it goes out of scope. The solution was to keep a reference of the timer throughout the application lifecycle.