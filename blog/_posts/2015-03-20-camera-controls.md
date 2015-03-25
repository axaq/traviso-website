---
layout: post
title: "Camera Controls"
date:   2015-03-20 10:00:00
summary: How to create camera controls for your traviso map.
categories: demo
---

___

Traviso has built-in camera methods that you can call externally to play around to get the best result in your own app logic.

At this point you can either read the tutorial below or go ahead and see the result immediately <a href="http://www.travisojs.com/examples/4/" target="_blank">here</a>.

<!--more-->

Here is a list of public methods that you can use with the engine:  

```js
centralizeToCurrentExternalCenter ( [instantRelocate=false] ) : External center is the central point of the frame defined by the user to be used as the visual size of the engine. This method centralizes the EngineView instance with respect to this external center-point.
centralizeToCurrentFocusLocation ( [instantRelocate=false] ) : Centralizes the EngineView instance to the current location of the attention/focus.
centralizeToLocation( c,  r,  [instantRelocate=false] ) : Centralizes the EngineView instance to the map location specified by row and column index.
centralizeToObject ( obj ) : Centralizes the EngineView instance to the object specified.
centralizeToPoint ( px,  py,  [instantRelocate=false] ) : Centralizes the EngineView instance to the point specified.
focusMapToLocation ( c,  r,  zoomAmount ) : Centralizes and zooms the EngineView instance to the map location specified by row and column index.
focusMapToObject ( obj ) : Centralizes and zooms the EngineView instance to the object specified.
setZoomParameters ( [minScale=0.5]  [maxScale=1.5]  [numberOfZoomLevels=5]  [initialZoomLevel=0]  [instantCameraZoom=false] ) : Sets all the parameters related to zooming in and out.
zoomOut ( [instantZoom=false] ) : Zooms the camera one level out.
zoomIn ( [instantZoom=false] ) : Zooms the camera one level in.
zoomTo ( zoomAmount  [instantZoom=false] ) : Zooms camera by to the amount given.
```

You can reach a more detailed documentation about this methods <a href="http://www.travisojs.com/docs/" target="_blank">here</a>.

Now, let's make a panel to control our camera.

First, we initialise a traviso engine. (For basic isometric engine creation you can read [this]({% post_url 2015-03-15-basic-isometric-world %} "Basic isometric world") tutorial.) 

```js
function update() 
{
    renderer.render(stage);
    requestAnimFrame(update); 
}

stage = new PIXI.Stage(0x6BACDE);
renderer = PIXI.autoDetectRenderer();
document.body.appendChild(renderer.view);

requestAnimFrame(update);

var instanceConfig =
{
    mapDataPath : "mapData.xml",
    assetsToLoad : ["grassTile.png", "house.png"],
};

var engine = TRAVISO.getEngineInstance(instanceConfig);
stage.addChild(engine);
```

Then, we need to create some images for the control buttons.

...... Images go here

Using the pixi logic we create sprites for our buttons:

```js
buttons
```

Now, all we need is to assign `onclick` methods for each one and link them to the engine methods that we want to play with. 

```js
onclick
```

<br/>
Finally, here is the entire code:

```js
final code
```

You can check out the demo from <a href="http://www.travisojs.com/examples/4/" target="_blank">here</a>.

Hope you liked it. Let me know your thoughts either in the comments below or through twitter.


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
