---
layout: post
title: "Data File Structure"
date:   2015-03-15 11:00:00
summary: A detailed explanation of the data file's structure.
categories: tutorial
---

___

Data file is a simple XML file which defines what goes where inside the engine.

<!--more-->

Here is an example:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<map_data>
	<tiles>
		<tile id="1" movable="1">grassTile.png</tile>
	</tiles>
	<objects>
	    <object id="1" movable="0" interactive="0" s="1x1">
			<v id="idle">
				<f>boxes.png</f>
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

So let's go step by step.

<br/>
### Tiles

`<tiles>` tag defines the available tiles that you can use in your map.

```xml
<tiles>
	<tile id="1" movable="1">images/grassTile.png</tile>
	<tile id="2" movable="0">images/waterTile.png</tile>
</tiles>
```

A `<tile>` tag should include the following attributes:

* **`id:`** (Numbers except 0) This is the key value that you will use to define the type of the tile. These will be used in the `<ground_map>` tag. DON'T use 0 as id since it represents empty tiles in the `<ground_map>`.

* **`movable:`** (0/1) This defines weather other characters can move onto this type of tile or not.
            
A `<tile>` tag should also include the image path/name as the tag value.

<br/>
### Objects

Similarly `<objects>` tag defines the available objects that you can use in your map.

```xml
<objects>
    <object id="1" movable="0" interactive="0" s="1x1">
		<v id="idle">
			<f>boxes.png</f>
		</v>
	</object>
</objects>
```

An `<object>` tag should include the following attributes:

* **`id:`** (Numbers except 0) This is the key value that you will use to define the type of the object. These will be used in the `<object_map>` tag. DON'T use 0 as id since it represents no-objects in the `<object_map>`.

* **`movable:`** (0/1) This defines weather other characters can move onto a tile that this objects sits on.

* **`interactive:`** (0/1) This specifies weather the user can select/interact with the object. The engine will use the callbacks to inform the developer once the object is selected by the user.

* **`s:`** This specifies the size of the object in the number of rows and colums (rows x colums).

An `<object>` tag should also include AT LEAST one `<v>` tag with `id="idle"`.
            
> **NOTE:** You can define as many visuals (single images or animation sequences) as you want using `<v>` tags.

Here is an animation defined for the idle state of an object. You can think of `<f>` tag as a frame for animation. If there is only one `<f>` tag then it is a still image instead of an animation.

```xml
<v id="idle">
    <f>hero_stand_1.png</f>
    <f>hero_stand_2.png</f>
    <f>hero_stand_3.png</f>
    <f>hero_stand_4.png</f>
    <f>hero_stand_5.png</f>
    <f>hero_stand_6.png</f>
</v>
```

You can create the textures/animations for your object in two ways:

* Either using child `<f>` tags to specify each frame texture if your image names are not in a numeric order like `walk1.png, walk2.png ...`
  
```xml
<v id="move_ne">
    <f>hero_move_ne_x.png</f>
    <f>hero_move_ne_w.png</f>
    <f>hero_move_ne_y.png</f>
    <f>hero_move_ne_z.png</f>
    <f>hero_move_ne_t.png</f>
    <f>hero_move_ne_v.png</f>
</v>
```
* Or in a single `<v>` tag by providing filename/path prefix and extension.

```xml  
<v id="move_ne" path="hero_move_ne_" ext="png" number_of_frames="6" start_number="1" /> 
```

<br/>
So, the following two `<v>` tags are interpreted as the same by the engine:
 
```xml
<v id="flip">
    <f>hero_flip_3.png</f>
    <f>hero_flip_4.png</f>
    <f>hero_flip_5.png</f>
    <f>hero_flip_6.png</f>
</v>
 
<v id="flip" path="hero_flip_" ext="png" number_of_frames="4" start_number="3" /> 
```

Here the attributes go as follows:

* **`id:`** Id of the texture or animation. You will use this to change object texture/animation any time.

* **`path:`** Image name/id as it is accessed throughout a cached spritesheet or with a path externally.

* **`ext:`** Image file extension. i.e. png, jpg

* **`number_of_frames:`** Number of frames in an animation. if it is not an animation but a single texture, set it to 1.

* **`start_number:`** Starting number of the image name suffix. This is only used for animations where image names are in a numeric order like walk1.png, walk2.png, walk3.png, walk4.png ...
                
> **NOTE 1:** Following ids are reserved, so for additional custom animations use different ids:

> `idle_s, idle_sw, idle_w, idle_nw, idle_n, idle_ne, idle_e, idle_se, move_s, move_sw, move_w, move_nw, move_n, move_ne, move_e, move_se`
	
> **NOTE 2:** `<f>` tags of a `<v>` tag has priority over attributes defining animation sequences. So if you use both the engine will process only the `<f>` tags.

> **NOTE 3:** For an object, at minimum, one `<v>` tag with the `id="idle"` is required. This will also be used as the initial visual (single texture or animation sequence) that will be applied to your object until you change it in your own logic.

<br/>
### Ground Map

`<ground_map>` tag defines ground/terrain layer of the map. `0` means no tile image AND non-walkable area.

```xml
<ground_map>
    <row>1 ,1 ,1 ,1</row>
    <row>1 ,0 ,0 ,1</row>
    <row>1 ,0 ,0 ,1</row>
    <row>1 ,1 ,1 ,1</row>
</ground_map>
<map_data>
```

This setting will create a frame of tiles with `id="1"` around an empty area with no tiles which means your controllable objects cannot walk on this area.

> **NOTE 1:** When used with `<single_ground_image>` tag, you don't necesserily need `<tiles>` tag. You can only include 0s and 1s to define which of the areas on the map are walkable. But you can also still put individual tile images on top of your global ground image by defining them in `<tiles>` tag.

> **NOTE 2:** If there is no `<single_ground_image>` defined then that means each tile has its own image which should be defined under `<tiles>` tag.

<br/>
### Single Ground Image `optional`

`<single_ground_image>` tag is an **optional** one. If you want to use a single ground/terrain image for your map instead of individual tile images then this property comes handy.

```xml
<single_ground_image scale="2">
    <path>assets/ground.jpg</path>
</single_ground_image>
```

When you define a `<path>` for your single-ground-image as shown above, then you can use `<ground_map>` tag just to define which areas are walkable and which are not on your map by using 0s and 1s.

The image should be loaded before the engine starts or it should be passed to the engine inside the `assetsToLoad` property of your configuration object.

> **NOTE:** If you are using a single-ground-image but also want individual tile images as well, then just go and define your tiles under `<tiles>` tag and use their ids in the `<ground_map>`. The engine will overlay individual tile images onto your single-ground-image. Just keep in mind that **no** tile can have `id="0"`.

`<single_ground_image>` tag only have one attribute at the moment:

* **`scale:`** Scale amount to apply to the single-ground-image so that you can use smaller images and scale them up for extra big maps, default 1.

<br/>
### Object Map

`<object_map>` tag defines object layer of the map. `0` means no object for that location.

```xml
<object_map>
    <row>0 ,0 ,0 ,0</row>
    <row>0 ,1 ,0 ,0</row>
    <row>0 ,0 ,1 ,0</row>
    <row>0 ,0 ,0 ,0</row>
</object_map>
```

Just put the proper id of the objects that you have defined under `<objects>` tag in the location you desire to put the object.

<br/>
### Image for Tile Highlighting `optional`

`<tile_highlight_image>` tag defines the image to be overlayed on the tile when highlighted.

```xml
<tile_highlight_image>tileHighlight.png</tile_highlight_image>
``` 

As always, either it should be loaded before the engine starts or it should be passed to the engine inside the `assetsToLoad` property of your configuration object. Use your tile size as a guide to create your highliting image. 

<br/>
### Initial Controllable Location `optional`


`<initial_controllable_location>` tag defines the location of the controllable object on the map when it is first initiated.

```xml
<initial_controllable_location c="5" r="10" />
```

This uses `c` and `r` attributes to define row and column indexes.

> **NOTE:** You don't need to include this tag in your data file if you want to define your controllable later on manually.

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
    	<i class="fa fa-lg fa-arrow-circle-right"></i>
    </a>
    {% endif %}
  </div>
</div>


