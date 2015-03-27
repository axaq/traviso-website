---
layout: post
title: "Using Single Ground Image"
date:   2015-03-27 10:00:00
summary: How to use a single image for the whole map ground/terrain.
categories: demo
---

___

With Traviso you can use a single ground image instead of using seperate tile images for each tile or you can mix the two.

To begin with, you need to set your XML data file correct. Optional `single_ground_image` tag should be set for this option.

<!--more-->

```xml
<single_ground_image scale="2">
    <path>assets/ground.png</path>
</single_ground_image>
```
When you define a `<path>` for your single-ground-image as shown above, then you can use `<ground_map>` tag just to define which areas are walkable and which are not on your map by using 0s and 1s.

```xml
<ground_map>
    <row>1 ,1 ,1 ,1</row>
    <row>1 ,0 ,0 ,1</row>
    <row>1 ,0 ,0 ,1</row>
    <row>1 ,1 ,1 ,1</row>
</ground_map>
<map_data>
```

The image should be loaded before the engine starts or it should be passed to the engine inside the `assetsToLoad` property of your configuration object.

```js
var instanceConfig =
{
    mapDataPath : "mapData.xml", // the path to the xml file that defines map data, required
    assetsToLoad : ["assets/ground.png", "house.png"], // array of paths to the assets that are desired to be loaded by traviso, no need to use if assets are already loaded to PIXI cache, default null
};

var engine = TRAVISO.getEngineInstance(instanceConfig);
```

If you are using a single-ground-image but also want individual tile images as well, then just go and define your tiles under `<tiles>` tag and use their ids in the `<ground_map>`. The engine will overlay individual tile images onto your single-ground-image. Just keep in mind that **no** tile can have `id="0"`.

```xml
<tiles>
	<tile id="1" movable="0">waterTile.png</tile>
	<tile id="2" movable="1">grassTile.png</tile>
</tiles>
<single_ground_image scale="2">
    <path>assets/ground.png</path>
</single_ground_image>
<ground_map>
    <row>1 ,1 ,1 ,0</row>
    <row>1 ,2 ,2 ,0</row>
    <row>1 ,2 ,2 ,0</row>
    <row>1 ,1 ,1 ,0</row>
</ground_map>
<map_data>
```

`<single_ground_image>` tag only have one attribute at the moment:

* **`scale:`** Scale amount to apply to the single-ground-image so that you can use smaller images and scale them up for extra big maps, default 1.

For a detailed explanation of the data file structure you can have a look at the related tutorial [here]({% post_url 2015-03-15-data-file-structure %} "Traviso data file structure").

<br/>
That's all for now. You can check out the demo from <a href="/examples/example6/" target="_blank">here</a>.

Hope you liked it. Let me know your thoughts either in the comments below or through twitter.


<br/>
<a href="https://github.com/axaq/traviso.js" target="_blank">Download</a> traviso and start playing around with the examples included.

Check out the documentation <a href="/docs/" target="_blank">here</a>.

<div id="post-navigation" >
  <div class="previous">
    {% if page.previous.url %}
    <a href="{{page.previous.url}}" title="Previous post: {{page.next.title}}">
      <i class="fa fa-lg fa-arrow-circle-left"></i>
      {{page.previous.title}}
    </a>
    {% endif %}
  </div>
  <div class="next text-right">
    {% if page.next.url %}
    <a href="{{page.next.url}}" title="Next post: {{page.next.title}}">
    	NEXT: {{page.next.title}}
    	<i class="fa fa-lg fa-arrow-circle-right"></i>
    </a>
    {% endif %}
  </div>
</div>
