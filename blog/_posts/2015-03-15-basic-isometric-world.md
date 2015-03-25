---
layout: post
title: "Basic Isometric World"
date:   2015-03-15 9:00:00
summary: Creating a simple isometric world.
---

___

Creating an isometric world with Traviso is pretty straightforward. Here is what u need:

1. Visuals for tiles and objects.
2. An XML file for Traviso to know what goes where.

> **NOTE:** Traviso is built on top of the pixi.js rendering engine. So if you haven't already check it out <a href="http://www.pixijs.com" target="_blank">here</a>.

<!--more-->

<br/>
### Creating a tile image

Isometric tiles are defined by two parameters throughout Traviso:

* **Iso-Height:** The height of a single tile.

  	<img src="/blog/images/posts/IsoHeight.png">
  	<br/>

* **Iso-Angle:** The angle between the isometric edge and the isometric (horizontal) diagnal of the tile.

  	<img src="/blog/images/posts/IsoAngle.png">


Default value for the Iso-Angle is 30 degrees and default height is 74 px. 

<br/>
### Creating an object image

While creating your objects better use a tile image as a reference to decide where your imeage goes on the tile.

Bottom corner of a tile is the reference point that traviso uses to align things inside the engine.

Just place your object-image on a tile-image, hide the tile-image, save it as the image file for your object.

<img src="/blog/images/posts/reference.png"><img src="/blog/images/posts/1x2object.png">

<br/>
### Creating the data file

Data file is a simple XML file which defines what goes where inside the engine.

Here is an example:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<map_data>
	<tiles>
		<tile id="1" movable="1">t_1.png</tile>
	</tiles>
	<objects>
	    <object id="1" movable="0" interactive="0" s="1x1">
			<v id="idle">
				<f>o_1.png</f>
			</v>
		</object>
	</objects>
	<ground_map>
		<row>1 ,1 ,1 ,1</row>
		<row>1 ,0 ,0 ,1</row>
		<row>1 ,0 ,0 ,1</row>
		<row>1 ,1 ,1 ,1</row>
	</ground_map>
	<object_map>
		<row>0 ,0 ,0 ,0</row>
		<row>0 ,1 ,0 ,0</row>
		<row>0 ,0 ,1 ,0</row>
		<row>0 ,0 ,0 ,0</row>
	</object_map>
<map_data>
```

For a detailed explanation of the data file structure you can have a look at the related tutorial [here]({% post_url 2015-03-15-data-file-structure %} "Traviso data file structure").
<br/>
### Ready to roll

So here is the code piece to initialise traviso:

```html
<script src="js/pixi.dev.js"></script>
<script src="js/traviso.dev.js"></script>

<script>

    ////// Global on-frame renderer function
    function update() 
    {
        renderer.render(stage);
        requestAnimFrame(update); 
    }
	
    ////// Here, we initialize the pixi stage
	
    // create an new instance of a pixi stage
    stage = new PIXI.Stage(0x6BACDE);

    // create a renderer instance
    renderer = PIXI.autoDetectRenderer();
	
    // add the renderer view element to the DOM
    document.body.appendChild(renderer.view);
	
    requestAnimFrame(update);
	
    ////// Here, we create our traviso instance and add on top of pixi
	
    // engine-instance configuration object
    var instanceConfig =
    {
        mapDataPath : "mapData.xml", // the path to the xml file that defines map data, required
        assetsToLoad : ["t_1.png", "o_1.png"], // array of paths to the assets that are desired to be loaded by traviso, no need to use if assets are already loaded to PIXI cache, default null
    };

    var engine = TRAVISO.getEngineInstance(instanceConfig);
    stage.addChild(engine);

</script>
```

Traviso aims to give you a lot of flexibility in terms of customisation. What you see above is the minimum and default settings. For a list of customisation items go ahead and read the related tutorial [here]({% post_url 2015-03-15-engine-configuration %} "Traviso engine configuration").

<br/>
<a href="https://github.com/axaq/traviso.js" target="_blank">Download</a> traviso and start playing around with the examples included.

Check out the documentation <a href="http://www.travisojs.com/docs/" target="_blank">here</a>.

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
    	<i class="fa fa-2x fa-arrow-circle-right"></i>
    </a>
    {% endif %}
  </div>
</div>

