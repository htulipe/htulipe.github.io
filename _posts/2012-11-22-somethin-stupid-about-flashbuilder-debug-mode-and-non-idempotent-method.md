--- 
layout: post
title: Somethin' stupid about Flashbuilder Debug mode and non idempotent method
tags: [Flex, Flash Builder]
category: Programming environment
type: post
meta: 
  _edit_last: "2"
  _syntaxhighlighter_encoded: "1"
  dsq_thread_id: "939572823"
---
Last day I waste about one hour on Adobe's <a title="AsDoc" href="http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/mx/utils/Base64Encoder.html" target="_blank">Base64Encoder</a> class. This class is the default Flash class if you want to encode a String in base64 and it works like this:


	var encoder:Base64Encoder = new Base64Encoder();
	encoder.encode("my string to encode");
	var result:String = encoder.toString();


And for me it simply did not work. While debugging with FlashBuilder, the toString() method returned an empty string.

Behind the stage, the class feeds a buffer every time the encode() method is called and the buffer is emptied when toString() is called. This is the critical information: the toString() method is NOT idempotent.

That fact was the reason my code was not working, but "Hey? I'm only calling the toString() method one time, right?". Answer: yes but not exactly... in my debug session I had a watch expression set to "encoder.toString()". So technically, since those watch expressions share the same environment as my application, the toString() method was called twice, once in the code, once in the watch expression... Funny part is that in non-debug mode, my code was working just fine.

This is what we can call a fail...
