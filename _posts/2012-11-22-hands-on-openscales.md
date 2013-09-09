--- 
layout: post
title: Hands on Openscales
tags: [Flex, Openscales]
category: Programming technic
type: post
meta: 
  _syntaxhighlighter_encoded: "1"
  _edit_last: "2"
  dsq_thread_id: "939772657"
  _wpas_done_all: "1"
---
Here is a really basic tutorial about Openscales 2.x, we do not have much of these on the Openscales community so I figured it could help.

This tutorial will lead you through a basic Flex application that use Openscales: this idea is to display a geographical layer.

I made this tutorial with Openscales 2.2 but it should work in any 2.x version. The IDE used is FlashBuilder 4.1.

Setting up the environment
--------------------------

Let's first download [Openscales](https://bitbucket.org/gis/openscales/downloads "Openscales download page") and unzip it.

Create a new project in Flash Builder (File > New > Flex project), be sure that is it defined as a Web Application (and not a Desktop Application). Then we need to add Openscales as a dependency, to do that copy/paste following artifacts from the zip archive to the "libs" directory of your project:

* openscales-core-2.2.swc
* openscales-fx-2.2.swc
* openscales-geometry-2.2.swc
* openscales-proj4as-2.2.swc

Ok, now you are set up, let's see what we have in that project.

A first map
-----------

The Main.mxml class is your Flash application, it should look like this:


	<xml version="1.0" encoding="utf-8"?>
	<s:Application xmlns:fx="http://ns.adobe.com/mxml/2009" xmlns:s="library://ns.adobe.com/flex/spark" xmlns:mx="library://ns.adobe.com/flex/mx" minWidth="955" minHeight="600">
		<fx:Declarations> <!-- ... --> </fx:Declarations>
	</s:Application>


This is where to start, let's first include openscales-fx package. Insert this namespace inside the Application tag:

`xmlns:os="http://openscales.org"`

Then insert a Map instance:


	<xml version="1.0" encoding="utf-8"?>
	<s:Application xmlns:fx="http://ns.adobe.com/mxml/2009" xmlns:s="library://ns.adobe.com/flex/spark" xmlns:mx="library://ns.adobe.com/flex/mx" xmlns:os="http://openscales.org" minWidth="955" minHeight="600">
		<os:Map id="map" width="100%" height="100%" resolution="0.2" center="4.833,45.767" projection="EPSG:900913">
			<os:Mapnik />
		</os:Map>
	</s:Application>


Now click the Execute button, you should see in your browser something like this:

![Alt "first screen shot"](/assets/images/hands-on-1.png)

This is the [Openstreet Map](http://www.openstreetmap.org/ "Openstreet map website")'s Mapnik layer that you see. How does that work? We created a map using the `Map` tag (corresponding to the `FxMap `Flex component) and added a `Mapnik` tag (`FxMapnik`) inside it. The FxMapnik is a layer class and since it's added inside the `Map` tag, it will be displayed. We also set several attributes such as:

`width` and `height`: Those tell which size the map should be in your application, they are Flex parameters.

`projection`: The geographical projection of the map, this will be the projection for ALL layers in the map, make sure the layers you use are compatible with that projection. Here we use EPSG:900913 which is also known as Google Mercator.

`resolution`: The resolution of the map is the number of pixel per geographic units (here degrees, since EPSG:900913 uses degrees). See it as the zoom level of your map. The resolution is calculated at the center of the map: `0.2` means that at the center of the map a pixel represent 0.2 degrees.

`center`: a (x,y) value specifying where the map should be centered using projection units (still degrees).

Interact with the map
---------------------

You might have notice that the map so far is undraggable and can't be zoomed. Let's change that by adding `Handlers`:


	<xml version="1.0" encoding="utf-8"?>
	<s:Application xmlns:fx="http://ns.adobe.com/mxml/2009" xmlns:s="library://ns.adobe.com/flex/spark" xmlns:mx="library://ns.adobe.com/flex/mx" xmlns:os="http://openscales.org" minWidth="955" minHeight="600">
		<os:Map id="map" width="100%" height="100%" resolution="0.2" center="4.833,45.767" projection="EPSG:900913">
			<os:Mapnik />
			<os:DragHandler/>
			<os:WheelHandler/>
		</os:Map>
	</s:Application>


Now if you execute the application, you can navigate by dragging the map and zoom with your mouse wheel. This is made possible by the `DragHandler` and the `WheelHandler` tags.

Adding another layer
--------------------

Ok now let's see how to add another layer. In Openscales you can overlay any number of layer you want as long as their projections are compatible. You can add WMS, WMTS, WFS, GPX, and others kind of layers (see the org.openscales.core.layer package). For now let's add a GPX layer.


	<xml version="1.0" encoding="utf-8"?>
	<s:Application xmlns:fx="http://ns.adobe.com/mxml/2009" xmlns:s="library://ns.adobe.com/flex/spark" xmlns:mx="library://ns.adobe.com/flex/mx" xmlns:os="http://openscales.org" minWidth="955" minHeight="600">
		<os:Map id="map" width="100%" height="100%" resolution="0.03" center="3.0,46.0" projection="EPSG:900913">
			<os:Mapnik />
			<os:GPX url="http://www.openscales.org/assets/simple_dep.gpx" />
			<os:DragHandler/>
			<os:KeyBoardHandler/>
			<os:WheelHandler/>
		</os:Map>
	</s:Application>


Note that we changed the `center` and `resolution` properties of the map. Now if you execute the application you will see the GPX data.

![Alt "second screen shot"](/assets/images/hands-on-2.png)

We simply added the `GPX` tag with the data location in the `url` property.

Okay this is it for this quick tutorial about Openscales but that's not all you can do. You can add another kind of layer or use the features drawing mechanism to create your own data or you could check out some useful controls. Etc etc...
