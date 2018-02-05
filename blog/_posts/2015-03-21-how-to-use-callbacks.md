---
layout: post
title: "How to Use Callback Properties"
date:   2015-03-21 10:00:00
summary: How to use callback methods to create your own logic.
categories: demo
demo: "/examples/example5/"
---

(Updated on Feb 03, 2018)
___

> **NOTE:** This document has been updated with the realese of <a href="https://github.com/axaq/traviso.js/releases" target="_blank">v1.0.0</a>. XML files and `callbackScope` setting are no longer in use.

Traviso has built-in callback methods that you can define through the configuration object that you pass to a traviso engine instance.

At this point you can either read the tutorial below or go ahead and see the result immediately <a href="/examples/example5/" target="_blank">here</a>.

<!--more-->

As always we create a traviso engine to start with:

```js
// engine-instance configuration object
var instanceConfig = {
    mapDataPath : "mapData.json", // the path to the json file that defines map data, required
    assetsToLoad : ["../assets/assets_map.json", "../assets/assets_characters.json"], // array of paths to the assets that are desired to be loaded by traviso, no need to use if assets are already loaded to PIXI cache, default null
    
	engineInstanceReadyCallback : onEngineInstanceReady, // callback function that will be called once everything is loaded and engine instance is ready, default null
	tileSelectCallback : onTileSelect, // callback function that will be called when a tile is selected, default null
	objectSelectCallback : onObjectSelect, // callback function that will be called when a tile with an interactive map-object on it is selected, default null
	objectReachedDestinationCallback : onObjectReachedDestination, // callback function that will be called when any moving object reaches its destination, default null
	otherObjectsOnTheNextTileCallback : onOtherObjectsOnTheNextTile // callback function that will be called when any moving object is in move and there are other objects on the next tile, default null
};

// create the engine
var engine = TRAVISO.getEngineInstance(instanceConfig);
```

> **NOTE:** We are telling the engine to load the necessary assets by defining them in the `assetsToLoad` property. Above we used json spritesheets to load all we need but you can also load the assets individually by defining them in the array the same way.

> **NOTE:** Note that you should bind your callback methods if you want to scope them under a certain js object.



Let's start by creating a method for each callback function. We start with `onEngineInstanceReady`

```js
function onEngineInstanceReady()
{
	stage.addChild(engine);
}
```

You can wait for the engine to setup and load all assets and when ready this callback will be called (if defined in the config object) so that you can add your engine to your view.

Secondly there is `tileSelectCallback` which is, as the name refers, being called when a tile is selected.

> **NOTE:** Keep in mind that if there is a controllable defined in the data file, when you click a tile, that controllable object will start moving towards that tile immediately.

Next, we can add some interactivity logic to our application. Here we will be using the flag object which is defined in our JSON data file as follows:

```json
"objects": {
    "10": {
        "movable": true,
        "interactive": true,
        "rowSpan": 1,
        "columnSpan": 1,
        "visuals": {
            "idle": {
                "frames": [
                    { "path": "o_flag_0.png" }
                ] 
            }
        }
    }
}
```

Notice that the `"interactive"` attribute should be `true` so that we can catch it with the `objectSelectCallback` method.

Let's say, when flag is clicked/touched by the user, we want our controllable to move to the flag and do a flip animation. The custom flip animation is also defined in the JSON data file:

```json
"flip": {
    "frames": [
        { "path": "hero_stand_se_0001.png" },
        { "path": "hero_stand_se_0001.png" },
        { "path": "hero_stand_sw_0001.png" },
        { "path": "hero_stand_sw_0001.png" },
        { "path": "hero_stand_nw_0001.png" },
        { "path": "hero_stand_nw_0001.png" },
        { "path": "hero_stand_ne_0001.png" },
        { "path": "hero_stand_ne_0001.png" },
        { "path": "hero_stand_se_0001.png" },
        { "path": "hero_stand_se_0001.png" }
    ]
}
```

In order to achieve this we create a callback function and send our moving object to the location of the flag:

```js
function onObjectSelect(obj)
{
	engine.moveCurrentControllableToLocation(obj.mapPos);
}
```

Here we used `moveCurrentControllableToLocation`. But alternatively, if your target object is something that you don't want your object to go onto, then you can use `moveCurrentControllableToObj` instead. This method will move the current controllable to one of the adjacent available tiles of the target object.

Next, we create another callback function which is being fired when any moving object reaches its destination. Within this function we can change the visual of the object to the desired flip animation by using its `id`.

```js
function onObjectReachedDestination(obj)
{
	var objectsOnDestination = engine.getObjectsAtLocation(obj.mapPos);
	for (var i=0; i < objectsOnDestination.length; i++)
	{
		// check if there is a flag on the destination tile
		if(objectsOnDestination[i].type === 10)
		{
			obj.changeVisual("flip", false, true, flipAnimFinished);
			break;
		}
	}
}

// this method will be called when the custom flip anim finished
function flipAnimFinished (obj)
{
	// change the visual of the object so that it will face its last direction
    obj.changeVisualToDirection(obj.currentDirection, false);
}
```

Notice that we also changed the objects' visual back to its stationary one so that it will face its last direction.

At this point, if you want to see it live, you can check out the demo <a href="/examples/example5/" target="_blank">here</a>.
<br/>

Next objective is to interact with the engine so that we will know what is on the way of the moving object and we can act accordingly.

We start with implementing the callback function for the `otherObjectsOnTheNextTileCallback`. 

```js
function onOtherObjectsOnTheNextTile(movingObject, objectsOnNewTile)
{
	var boxAnim;
	for (var i=0; i < objectsOnNewTile.length; i++)
	{
		// check if there are boxes on the next tile
		if(objectsOnNewTile[i].type === 12)
		{
			boxAnim = createAndStartBoxAnim();
			engine.addCustomObjectToLocation(boxAnim, objectsOnNewTile[i].mapPos);
			engine.removeObjectFromLocation(objectsOnNewTile[i]);
		}
	}
}
```

As you see, this callback method sends both the moving object and a list of objects that are on the next tile in the way. We are interested in the pile-of-box object so we check for its `id` which is `12` in our case. 

Finally, we will add a method to create little boxes and spread them up:

```js
function createAndStartBoxAnim()
{
	// create six seperate boxes to spread
    var boxAnim = new PIXI.Container();
    var t = PIXI.Texture.fromFrame("box.png");
    var box;
    var boxes = [];
    for (var i=0; i < 6; i++)
    {
        box = new PIXI.extras.AnimatedSprite([t]);
        box.anchor.x = box.anchor.y = 0.5;
        boxAnim.addChild(box);
        boxes[boxes.length] = box;
    }
    
    // play a simple spread anim for the boxes
    var boxTargets = [[-75, -125], [-50, -100], [-25, -75], [25, -75], [50, -100], [75, -125]];
    for (var i=0; i < boxes.length; i++)
    {
        TweenLite.to( boxes[i].position, 0.5, { x: boxTargets[i][0], y: boxTargets[i][1], ease:"Back.easeOut" } );
        TweenLite.to( boxes[i], 0.7, { alpha: 0 } );
    }
    
    return boxAnim;
}
```

That's all for now. You can check out the demo from <a href="/examples/example5/" target="_blank">here</a>.

Above examples demostrate just a way of using the callbacks but you can come up with different uses depending on your logic.

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
