---
layout: post
title: "Camera Controls"
date:   2015-03-20 10:00:00
summary: How to create camera controls for your traviso map.
categories: demo
demo: "/examples/example4/"
---

(Updated on Feb 03, 2018)

___

> **NOTE:** This document has been updated with the realese of <a href="https://github.com/axaq/traviso.js/releases" target="_blank">v1.0.0</a>. XML files are no longer in use and instead we have json files for map data.

Traviso has built-in camera methods that you can call externally to play around to get the best result in your own app logic.

At this point you can either read the tutorial below or go ahead and see the result immediately <a href="/examples/example4/" target="_blank">here</a>.

<!--more-->

Here is a list of public methods realted to camera controls that you can use with the engine:  


* `centralizeToCurrentExternalCenter( instantRelocate=false )` : External center is the central point of the frame defined by the user to be used as the visual size of the engine. This method centralizes the EngineView instance with respect to this external center-point.
* `centralizeToCurrentFocusLocation( instantRelocate=false )` : Centralizes the EngineView instance to the current location of the attention/focus.
* `centralizeToLocation( c,  r,  instantRelocate=false )` : Centralizes the EngineView instance to the map location specified by row and column index.
* `centralizeToObject( obj )` : Centralizes the EngineView instance to the object specified.
* `centralizeToPoint( px,  py,  instantRelocate=false )` : Centralizes the EngineView instance to the point specified.
* `focusMapToLocation( c,  r,  zoomAmount )` : Centralizes and zooms the EngineView instance to the map location specified by row and column index.
* `focusMapToObject( obj )` : Centralizes and zooms the EngineView instance to the object specified.
* `setZoomParameters( minScale=0.5,  maxScale=1.5,  numberOfZoomLevels=5,  initialZoomLevel=0,  instantCameraZoom=false )` : Sets all the parameters related to zooming in and out.
* `zoomOut( instantZoom=false )` : Zooms the camera one level out.
* `zoomIn( instantZoom=false )` : Zooms the camera one level in.
* `zoomTo( zoomAmount,  instantZoom=false )` : Zooms camera by to the amount given.


You can reach a more detailed documentation about this methods <a href="/docs/" target="_blank">here</a>.

Now, let's make a panel to control our camera.

First, we initialise a traviso engine. (For basic isometric engine creation you can read [this]({% post_url 2015-03-15-basic-isometric-world %} "Basic isometric world") tutorial.) 

```js
////// Here, we initialize the pixi application
var pixiRoot = new PIXI.Application(800, 600, { backgroundColor : 0x6BACDE });

// add the renderer view element to the DOM
document.body.appendChild(pixiRoot.view);

////// Here, we create our traviso instance and add on top of pixi

// engine-instance configuration object
var instanceConfig = {
    mapDataPath : "mapData.xml",
    assetsToLoad : ["grassTile.png", "house.png"],
    initialPositionFrame: { x : 0, y : 0, w : 800, h : 600 },
    engineInstanceReadyCallback : onEngineInstanceReady
};

var engine = TRAVISO.getEngineInstance(instanceConfig);

// this method will be called when the engine is ready
function onEngineInstanceReady () {
    pixiRoot.stage.addChild(engine);
}
```

Then, we need to create some images for the control buttons.

<img src="/blog/images/posts/btn_zoomIn.png"><img src="/blog/images/posts/btn_zoomOut.png"><img src="/blog/images/posts/btn_centralize.png"><img src="/blog/images/posts/btn_centralizeToObject.png"><img src="/blog/images/posts/btn_focusToObject.png">

Using the pixi logic we create sprites for our buttons:

```js
function onEngineInstanceReady() {
    pixiRoot.stage.addChild(engine);
        
    // create buttons
    var btnZoomIn = new PIXI.Sprite.fromFrame("../assets/btn_zoomIn.png");
    pixiRoot.stage.addChild(btnZoomIn);
    
    var btnZoomOut = new PIXI.Sprite.fromFrame("../assets/btn_zoomOut.png");
    pixiRoot.stage.addChild(btnZoomOut);
    
    var btnCentralize = new PIXI.Sprite.fromFrame("../assets/btn_centralize.png");
    pixiRoot.stage.addChild(btnCentralize);
    
    var btnCentralizeToObject = new PIXI.Sprite.fromFrame("../assets/btn_centralizeToObject.png");
    pixiRoot.stage.addChild(btnCentralizeToObject);
    
    var btnFocusMapToObject = new PIXI.Sprite.fromFrame("../assets/btn_focusToObject.png");
    pixiRoot.stage.addChild(btnFocusMapToObject);
    
    // set positions
    btnZoomIn.position.x = 8;
    btnZoomOut.position.x = btnZoomIn.position.x + btnZoomIn.width + 8;
    btnCentralize.position.x = btnZoomOut.position.x + btnZoomOut.width + 8;
    btnCentralizeToObject.position.x = btnCentralize.position.x + btnCentralize.width + 8;
    btnFocusMapToObject.position.x = btnCentralizeToObject.position.x + btnCentralizeToObject.width + 8;
}
```

Now, all we need is to assign `click` methods for each one and link them to the engine methods that we want to play with. 

```js
btnZoomIn.interactive = btnZoomIn.buttonMode = true;
btnZoomOut.interactive = btnZoomOut.buttonMode = true;
btnCentralize.interactive = btnCentralize.buttonMode = true;
btnCentralizeToObject.interactive = btnCentralizeToObject.buttonMode = true;
btnFocusMapToObject.interactive = btnFocusMapToObject.buttonMode = true;

// add click callbacks
btnZoomIn.click = btnZoomIn.tap = function(data) {
    engine.zoomIn();
};

btnZoomOut.click = btnZoomOut.tap = function(data) {
    engine.zoomOut();
};

btnCentralize.click = btnCentralize.tap = function(data) {
    engine.centralizeToCurrentExternalCenter();
};

btnCentralizeToObject.click = btnCentralizeToObject.tap = function(data) {
    engine.centralizeToObject(engine.getCurrentControllable());
};

btnFocusMapToObject.click = btnFocusMapToObject.tap = function(data) {
    engine.focusMapToObject(engine.getCurrentControllable());
};
```

Here `zoomIn`, `zoomOut`, `centralizeToObject` are pretty straightforward.

`centralizeToCurrentExternalCenter` centralizes the engine camera with respect to this external center-point. External center is the central point of the frame (defined in the config object or `{ x : 0, y : 0, w : 800, h : 600 }` by default) used as the visual size of the engine.

`focusMapToObject` method, on the other hand, both centralizes and zooms the engine camera to the object specified. Here focus means zooming in so you need to zoom out a little bit to see the correct effect.

<br/>
Finally, here is the entire code:

```js
var pixiRoot = new PIXI.Application(800, 600, { backgroundColor : 0x6BACDE });
document.body.appendChild(pixiRoot.view);

var instanceConfig = {
    mapDataPath : "mapData.xml",
    assetsToLoad : ["grassTile.png", "house.png"],
    initialPositionFrame: { x : 0, y : 0, w : 800, h : 600 },
    engineInstanceReadyCallback : onEngineInstanceReady
};
var engine = TRAVISO.getEngineInstance(instanceConfig);

function onEngineInstanceReady () {
    pixiRoot.stage.addChild(engine);
        
    // create buttons
    var btnZoomIn = new PIXI.Sprite.fromFrame("../assets/btn_zoomIn.png");
    pixiRoot.stage.addChild(btnZoomIn);
    
    var btnZoomOut = new PIXI.Sprite.fromFrame("../assets/btn_zoomOut.png");
    pixiRoot.stage.addChild(btnZoomOut);
    
    var btnCentralize = new PIXI.Sprite.fromFrame("../assets/btn_centralize.png");
    pixiRoot.stage.addChild(btnCentralize);
    
    var btnCentralizeToObject = new PIXI.Sprite.fromFrame("../assets/btn_centralizeToObject.png");
    pixiRoot.stage.addChild(btnCentralizeToObject);
    
    var btnFocusMapToObject = new PIXI.Sprite.fromFrame("../assets/btn_focusToObject.png");
    pixiRoot.stage.addChild(btnFocusMapToObject);
    
    // set positions
    btnZoomIn.position.x = 8;
    btnZoomOut.position.x = btnZoomIn.position.x + btnZoomIn.width + 8;
    btnCentralize.position.x = btnZoomOut.position.x + btnZoomOut.width + 8;
    btnCentralizeToObject.position.x = btnCentralize.position.x + btnCentralize.width + 8;
    btnFocusMapToObject.position.x = btnCentralizeToObject.position.x + btnCentralizeToObject.width + 8;

    btnZoomIn.interactive = btnZoomIn.buttonMode = true;
    btnZoomOut.interactive = btnZoomOut.buttonMode = true;
    btnCentralize.interactive = btnCentralize.buttonMode = true;
    btnCentralizeToObject.interactive = btnCentralizeToObject.buttonMode = true;
    btnFocusMapToObject.interactive = btnFocusMapToObject.buttonMode = true;

    // add click callbacks
    btnZoomIn.click = btnZoomIn.tap = function(data) {
        engine.zoomIn();
    };

    btnZoomOut.click = btnZoomOut.tap = function(data) {
        engine.zoomOut();
    };

    btnCentralize.click = btnCentralize.tap = function(data) {
        engine.centralizeToCurrentExternalCenter();
    };

    btnCentralizeToObject.click = btnCentralizeToObject.tap = function(data) {
        engine.centralizeToObject(engine.getCurrentControllable());
    };

    btnFocusMapToObject.click = btnFocusMapToObject.tap = function(data) {
        engine.focusMapToObject(engine.getCurrentControllable());
    };
}
```

You can check out the demo from <a href="/examples/example4/" target="_blank">here</a>.


<br/>
Additionally there are some handy properties related to camera controls that you can define in the confuguration object. Here is a list:

* **minScale {Number} :** mimimum scale that the DisplayObjectContainer for the map can get, default 0.5
* **maxScale {Number} :** maximum scale that the DisplayObjectContainer for the map can get, default 1.5
* **numberOfZoomLevels {Number} :** used to calculate zoom increment, default 5
* **initialZoomLevel {Number} :** initial zoom level of the map, should be between -1 and 1, default 0
* **instantCameraZoom {Number} :** specifies wheather to zoom instantly or with a tween animation, default false
* **followCharacter {Boolean} :** defines if the camera will follow the current controllable or not, default true
* **instantCameraRelocation {Boolean} :** specifies wheather the camera moves instantly or with a tween animation to the target location, default false
* **mapDraggable {Boolean} :** enable dragging the map with touch-and-touchmove or mousedown-and-mousemove on the map, default true

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
