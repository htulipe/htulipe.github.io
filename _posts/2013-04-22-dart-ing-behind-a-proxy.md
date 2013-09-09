--- 
layout: post
title: Dart-ing behind a proxy
tags: [Dart, proxy]
category: Programming environment

status: publish
type: post
published: true
meta: 
  dsq_thread_id: "1228181029"
  _wpas_done_all: "1"
  _syntaxhighlighter_encoded: "1"
  _edit_last: "2"
---
I just started looking at <a title="Google Dart" href="https://www.dartlang.org/">Dart</a> and ran in a trouble that may concern other developers. While testing the base tutorial (<a title="clickme webapp" href="https://www.dartlang.org/docs/tutorials/get-started/#create-web-app">clickme webapp</a>) provided by the Dart guys, I got an error:

	Pub install failed, 
	[1] Resolving dependencies... Got socket error trying to find package "browser" at https://pub.dartlang.org

Of course the app was not working, because pub could not resolve the one dependency of this sample project.

This was due to a proxy in my company. The big problem is that Dart Editor does not provide (in M4 at least) a way to configure a proxy. The solution is to run pub manually because it will take into account your env variables. Here is how to do it:

First, the pub server is behing HTTPS and there is a bug (filled <a href="http://code.google.com/p/dart/issues/detail?id=5454">here</a>) that will cause https request to fail whatever your configuration is so we will have to use HTTP.

Be sure you have your HTTP_PROXY environment variable defined to your proxy and then go to the hosted_srouce.dart file located here:

`your_dart_sdk_path\util\pub\hosted_source.dart`

Look for the final variable called `_defaultUrl` (line 155) and change `https` to `http`

`final _defaultUrl = "http://pub.dartlang.org";`

Now go to your clickme root dir in a terminal and run

	pub install

The dependency should be resolved and you can now run you app from Dart Editor.
