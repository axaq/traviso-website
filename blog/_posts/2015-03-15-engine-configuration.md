---
layout: post
title: "Engine Configuration"
date:   2015-03-15 10:00:00
summary: How to configure your traviso engine instance.
categories: tutorial
---

___

When you create a traviso engine instance you need to define a configuration object for it.

```js
var instanceConfig =
{
    mapDataPath : "mapData.xml",
    assetsToLoad : ["grassTile.png", "waterTile.png", "house.png", "box.png"],
};

var engine = TRAVISO.getEngineInstance(instanceConfig);
```

<!--more-->

`mapDataPath` is the only required component which specifies the path to the XML file that defines map data.

`assetsToLoad` is an array of paths to the assets that are desired to be loaded by traviso. No need to use this if your assets are already loaded to PIXI cache.

Here is a breakdown of all the other parameters that you can playaround to customise your isometric engine.

```js
mapDataPath {String} // the path to the xml file that defines map data, required
assetsToLoad {Array(String)} // array of paths to the assets that are desired to be loaded by traviso, no need to use if assets are already loaded to PIXI cache, default null

minScale {Number} // mimimum scale that the DisplayObjectContainer for the map can get, default 0.5
maxScale {Number} // maximum scale that the DisplayObjectContainer for the map can get, default 1.5
numberOfZoomLevels {Number} // used to calculate zoom increment, default 5
initialZoomLevel {Number} // initial zoom level of the map, should be between -1 and 1, default 0
instantCameraZoom {Number} // specifies wheather to zoom instantly or with a tween animation, default false

tileHeight {Number} // height of a single isometric tile, default 74
isoAngle {Number} // the angle between the topleft edge and the horizontal diagonal of a isometric quad, default 30

initialPositionFrame {Object} // frame to position the engine, default { x : 0, y : 0, w : 800, h : 600 }
initialPositionFrame.x {Number} // x position of the engine, default 0
initialPositionFrame.y {Number} // y position of the engine, default 0
initialPositionFrame.w {Number} // width of the engine, default 800
initialPositionFrame.h {Number} // height of the engine, default 600

pathFindingType {Number} // the type of path finding algorithm two use, default TRAVISO.pfAlgorithms.ASTAR_ORTHOGONAL

followCharacter {Boolean} // defines if the camera will follow the current controllable or not, default true
instantCameraRelocation {Boolean} // specifies wheather the camera moves instantly or with a tween animation to the target location, default false
instantObjectRelocation {Boolean} // specifies wheather the map-objects will be moved to target location instantly or with an animation, default false

highlightPath {Boolean} // highlight the path when the current controllable moves on the map, default true
highlightTargetTile {Boolean} // highlight the target tile when the current controllable moves on the map, default true
tileHighlightAnimated {Boolean} // animate the tile highlights, default true

mapDraggable {Boolean} // enable dragging the map with touch-and-touchmove or mousedown-and-mousemove on the map, default true

backgroundColor {Number(Hexadecimal)} // background color, if defined the engine will create a solid colored background for the map, default null
useMask {Boolean} // creates a mask using the position frame defined by 'initialPositionFrame' property or the 'posFrame' parameter that is passed to 'repositionContent' method, default false

callbackScope {Object} // the scope to apply when calling callback functions, default null
engineInstanceReadyCallback {Function} // callback function that will be called once everything is loaded and engine instance is ready, needs 'callbackScope' property, default null
tileSelectCallback {Function} // callback function that will be called when a tile is selected, needs 'callbackScope' property, default null
objectSelectCallback {Function} // callback function that will be called when a tile with an interactive map-object on it is selected, needs 'callbackScope' property, default null
objectReachedDestinationCallback {Function} // callback function that will be called when any moving object reaches its destination, needs 'callbackScope' property, default null
otherObjectsOnTheNextTileCallback {Function} // callback function that will be called when any moving object is in move and there are other objects on the next tile, needs 'callbackScope' property, default null
```

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
