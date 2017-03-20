---
published: false
layout: post
subtitle: asp.net core identity without entity framework
date: 2017-03-20T12:23:33.000Z
author: Taher Chhabra
header-img: img/posts/Simple-Blue-Background.jpg
comments: true
tags:
  - aspnetcore
---
I was trying to setup the new Asp.net Core Identity without Entity framework. Googled a bit and found these stackoverflow answers

http://stackoverflow.com/questions/36095076/custom-authentication-in-asp-net-core http://stackoverflow.com/questions/31271600/asp-net-5-identity-custom-signinmanager http://stackoverflow.com/questions/36184856/authentication-and-authorization-without-entity-framework-in-asp-net-5-mvc-6

All the answers hints at implementing Asp.net Identity Interfaces and making changes in startup.cs. But none of these answers were working for me. Googled a bit more and found [this](http://stackoverflow.com/questions/36794285/how-to-use-asp-net-core-1-signinmanager-without-entityframework). There is a [sample](https://github.com/MatthewKing/IdentityWithoutEF) project in the answer which helped a lot even though it doesn’t work with the latest asp.net version. Also after implementing the interfaces using the sample project SignInManager.IsSignedIn(User) method was always returning false. Found the fix [here](https://github.com/aspnet/Home/issues/1727)

In all you just need to implement IRoleStore, IUserStore and create CustomUser and CustomRole class and add these lines in Startup.cs

    services.AddSingleton<IUserStore<ApplicationUser>, ExampleUserStore>();
    services.AddSingleton<IRoleStore<ApplicationRole>, ExampleRoleStore>();
    services.AddIdentity<ApplicationUser, ApplicationRole>().AddDefaultTokenProviders();
    
Here is the working [sample](https://github.com/taherchhabra/AspNetCoreIdentityWithoutEF) of Asp.Net core without EF