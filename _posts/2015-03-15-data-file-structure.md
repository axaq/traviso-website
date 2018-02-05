---
layout: post
title: "Data File Structure"
date:   2015-03-15 11:00:00
summary: A detailed explanation of the data file's structure.
categories: tutorial
---

(Updated on Feb 03, 2018)
___

> **NOTE:** This document has been updated with the realese of <a href="https://github.com/axaq/traviso.js/releases" target="_blank">v1.0.0</a>. XML files are no longer in use and instead we have json files for map data.

Data file is a simple XML file which defines what goes where inside the engine.

<!--more-->

Here is an example:

```json
{
    "tiles": {
        "1":  {
            "movable": true,
            "path": "grassTile.png"
        }
    },
    "objects": {
        "1": {
            "movable": false,
            "interactive": false,
            "rowSpan": 1,
            "columnSpan": 1,
            "visuals": {
                "idle": {
                    "frames": [
                        { "path": "boxes.png" }
                    ] 
                }
            }
        }
    },
    "groundMap": [
        { "row": "1 ,1 ,1 ,1" },
        { "row": "1 ,0 ,0 ,1" },
        { "row": "1 ,0 ,0 ,1" },
        { "row": "1 ,1 ,1 ,1" }
    ],
    "objectsMap": [
        { "row": "0 ,0 ,0 ,0" },
        { "row": "0 ,1 ,0 ,0" },
        { "row": "0 ,0 ,1 ,0" },
        { "row": "0 ,0 ,0 ,0" }
    ]
}
```

So let's go step by step.

<br/>
### Tiles

`"tiles"` section defines the available tiles that you can use in your map.

```json
"tiles": {
    "1":  { "movable": true, "path": "grassTile.png" },
    "2":  { "movable": false, "path": "waterTile.png" }
}
```

A tile item is keyed by its id. These ids will be used in the `"groundMap"` later in the data file to identify the location of the tile. DON'T use `"0"` as id/key since it represents empty tiles in the `"groundMap"`.

A tile should include the following attributes:

* **`movable:`** (true/false) This defines weather other characters can move onto this type of tile.

* **`path:`** (String) Image name/id as it is accessed throughout a cached spritesheet or with a path externally.

<br/>
### Objects

Similarly `"objects"` section defines the available objects that you can use in your map.

```json
"objects": {
    "1": {
        "movable": false,
        "interactive": false,
        "rowSpan": 1,
        "columnSpan": 1,
        "noTransparency": true,
        "floor": false,
        "visuals": {
            "idle": {
                "frames": [
                    { "path": "boxes.png" }
                ]
            }
        }
    }
}
```

An object item is keyed by its id. These ids will be used in the `"objectsMap"` later in this file to identify location of the object. DON'T use `"0"` as id/key since it represents the case of no-objects in the `"objectsMap"`.

An object item should include the following attributes:

* **`movable:`** (true/false) This defines weather other characters can move onto a tile that this object sits on.

* **`interactive:`** (true/false) This specifies weather the user can select/interact with the object. The engine will use the callbacks to inform the developer once the object is selected by the user.

* **`rowSpan:`** (Integer 0<) This specifies the size of the object in terms of number of rows it allocates.

* **`columnSpan:`** (Integer 0<) This specifies the size of the object in terms of number of columns it allocates.

* **`noTransparency:`** (true/false) This specifies if the engine will make the object transperent or not when the main controller is behind it.

* **`floor:`** (true/false) This specifies if the object is a floor-object (like a rug) or not.

* **`visuals:`** (Object) This is the container that holds all the visuals available for the map object. It should include AT LEAST one visual with the id/key `"idle"`.
            
> **NOTE:** You can define as many visuals (single images or animation sequences) as you want inside the `"visuals"` section.

Here is an animation defined for the idle state of an object. You can think of each row inside the `"frames"` array as a frame for animation. If there is only one row then it is a still image instead of an animation.

```json
"idle": {
    "frames": [
        { "path": "hero_stand_1.png" },
        { "path": "hero_stand_2.png" },
        { "path": "hero_stand_3.png" },
        { "path": "hero_stand_4.png" },
        { "path": "hero_stand_5.png" },
        { "path": "hero_stand_6.png" }
    ]
}
```

You can create the textures/animations for your object in two ways:

* Either using `"frames"` property to specify each frame texture if your image names are not in a numeric order like `walk1.png, walk2.png ...`

```json
"move_ne": {
    "frames": [
        { "path": "hero_move_ne_x.png" },
        { "path": "hero_move_ne_w.png" },
        { "path": "hero_move_ne_y.png" },
        { "path": "hero_move_ne_z.png" },
        { "path": "hero_move_ne_t.png" },
        { "path": "hero_move_ne_v.png" }
    ]
}
```

* Or in a single visual by providing `"path"` (prefix), `"startIndex"`, `"numberOfFrames"` and `"extension"`.

```json  
"move_ne": { "path": "hero_move_ne_", "startIndex": 1, "numberOfFrames": 6, "extension": "png" }
```

<br/>
So, the following two visulas are interpreted as the same by the engine:
 
```json
"flip": {
    "frames": [
        { "path": "hero_flip_3.png" },
        { "path": "hero_flip_4.png" },
        { "path": "hero_flip_5.png" },
        { "path": "hero_flip_6.png" }
    ]
}

"flip": { "path": "hero_flip_", "startIndex": 3, "numberOfFrames": 4, "extension": "png" }
```

Here the attributes go as follows:

* **`path:`** Image name/id as it is accessed throughout a cached spritesheet or with a path externally.

* **`extension:`** Image file extension. i.e. png, jpg

* **`numberOfFrames:`** (Integer 0<) Number of frames in an animation. If it is not an animation but a single texture, set it to 1.

* **`startIndex:`** (Integer) Starting number of the image name suffix. This is only used for animations where image names are in a numeric order like walk3.png, walk4.png ...

* **`ipoc:`** (Integer) Interaction-point column-offset for the visual.

* **`ipor:`** (Integer) Interaction-point row-offset for the visual.

If you want to create an interaction point for a visual you can set interaction-point-offset by defining 'ipor' and 'ipoc' as row and column. This means when the you use methods like 'moveControllableToObj' the engine will move the controllable to this interaction points instead of just the nearest neighbouring tile. For instance, you can have characters interacting and you can allow your controllable character to interact with other characters depending on which way they are looking.
                
> **NOTE 1:** Following ids are reserved, so for additional custom animations use different ids:

> `idle_s, idle_sw, idle_w, idle_nw, idle_n, idle_ne, idle_e, idle_se, move_s, move_sw, move_w, move_nw, move_n, move_ne, move_e, move_se`
	
> **NOTE 2:** `"frames"` of a visual has priority over attributes defining animation sequences. So if you use both the engine will process only the `"frames"` property.

> **NOTE 3:** For an object, at minimum, one visual with the id `"idle"` is required. This will also be used as the initial visual (single texture or animation sequence) that will be applied to your object until you change it in your own logic. 

<br/>
### Ground Map

`"groundMap"` section defines ground/terrain layer of the map. `0` means no tile image AND non-walkable area.

```json
"groundMap": [
    { "row": "1 ,1 ,1 ,1" },
    { "row": "1 ,0 ,0 ,1" },
    { "row": "1 ,0 ,0 ,1" },
    { "row": "1 ,1 ,1 ,1" }
]
```

This setting will create a frame of tiles with type `1` around an empty area with no tiles which means your controllable objects cannot walk on this area.

> **NOTE 1:** When used with `"singleGroundImage"`, you don't necesserily need "tiles" section. You can only include `0`s and `1`s to define which of the areas on the map are walkable. But you can also still put individual tile images on top of your global ground image by defining them in `"tiles"` section.

> **NOTE 2:** If there is no `"singleGroundImage"` defined then that means each tile has its own image which should be defined under `"tiles"` section.

<br/>
### Single Ground Image `optional`

`"singleGroundImage"` section is an **optional** one. If you want to use a single ground/terrain image for your map instead of individual tile images then this property comes handy.

```json
"singleGroundImage": { "path": "assets/ground.jpg", "scale": 2 }
```

When you define a `"path"` for your single-ground-image as shown above, then you can use `"groundMap"` section just to define which areas are walkable and which are not on your map by using `0`s and `1`s.

The image should be loaded before the engine starts or it should be passed to the engine inside the `assetsToLoad` property of your configuration object.

> **NOTE:** If you are using a single-ground-image but also want individual tile images as well, then just go and define your tiles under `"tiles"` section and use their ids in the `"groundMap"`. The engine will overlay individual tile images onto your single-ground-image. Just keep in mind that **no** tile can have key/id `"0"`.

`"singleGroundImage"` section only have two attributes at the moment:

* **`path:`** Image name/id as it is accessed throughout a cached spritesheet or with a path externally.

* **`scale:`** Scale amount to apply to the single-ground-image so that you can use smaller images and scale them up for extra big maps, default 1.

<br/>
### Object Map

`"objectsMap"` section defines object layer of the map. `0` means no object for that location.

```json
"objectsMap": [
    { "row": "0 ,0 ,0 ,0" },
    { "row": "0 ,1 ,0 ,0" },
    { "row": "0 ,0 ,1 ,0" },
    { "row": "0 ,0 ,0 ,0" }
]
```

Just put the proper id of the objects that you have defined under `"objects"` section in the location you desire to put the object.

<br/>
### Image for Tile Highlighting `optional`

`"tileHighlightImage"` tag defines the image to be overlayed on the tile when highlighted.

```json
"tileHighlightImage": { "path": "tileHighlight.png" }
``` 

As always, either it should be loaded before the engine starts or it should be passed to the engine inside the `assetsToLoad` property of your configuration object. Use your tile size as a guide to create your highliting image.

<br/>
### Initial Controllable Location `optional`

`"initialControllableLocation"` section defines the location of the controllable object on the map when it is first initiated.

```json
"initialControllableLocation": { "columnIndex": 5, "rowIndex": 10 }
```

This uses `columnIndex` and `rowIndex` attributes to define row and column indexes.

> **NOTE:** You don't need to include this tag in your data file if you want to define your controllable later on manually.

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


