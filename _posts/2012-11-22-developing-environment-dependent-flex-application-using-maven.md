--- 
layout: post
title: Developing environment dependent Flex application using Maven
tags: [Programming technic]

status: draft
type: post
published: false
meta: 
  _edit_last: "2"
---
In most projects I have worked on, we use environments in our build process. Mostly it's qualification, pre-production and production. When a new feature come's out of development, it is first released on qualification (for testing purposes), then pre-production (technically equal to production) to finally end up in production <span style="color: #888888;">environment</span>. Since those environments correspond to machines, web services (...) there is a need to pull out of the code any environment specific information (machine name, web service URL, ...).

For that, Java provides cools way of having environment dependent properties files (.properties) that are loaded and read at runtime by a Java web app (for instance). Another great advantage of such mechanism is that when a web service URL has changed (for whatever reason), you do not need to recompile your Java application. You basically have to change the .properties files.

In a Flex application we can do practically the same despite sandbox policies (Flash will not allow an external file to be loaded inside your SWF): you can load whatever you want that is located in the same directory (or subdirectory) of your SWF. Hence, if you have such a file layout:

- MyApp.swf

- conf/

- conf/config.properties

Then, loading config.properties form MyApp.swf will not raise an exception, since its in a subdirectory of the application. This is true not only in a local context but also when your application is on hosted a server.

In the case of our application when can imagine having 3 properties files (one for each environment) and when we want to push the app in a specific env, we simply push the SWF and the correct property file. All of the environment knowledge will be held inside the properties file and the SWF will stay environment agnostic. Isn't that funky?

What would be more funky is to integrate this in our Maven build process. To have for each release, the three properties in a separated artifact along with the SWF. And those artifacts would be pushed to our repository, ready to be delivered.

So now I will talk about that, I will talk about <strong>Maven assemblies</strong>. Assemblies are a powerful Maven feature that allows you to generated any kind of archive from you project content. You just have to describe what you want to archive in a XML file called a <strong>descriptor</strong>.

Note that the rest on this post can be applied to any Maven based project, either it use Flex  or Java or whatever language.

So what we want to do is create 3 descriptors files, one for each environment:
