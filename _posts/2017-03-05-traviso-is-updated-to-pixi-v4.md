---
layout: post
title: "Traviso is updated to Pixi v4"
date:   2017-03-05 10:00:00
summary: Traviso.js is now updated with the latest version (v4.4.1) of Pixi.js.
categories: update
---

___

Traviso.js is now updated with the latest version (v4.4.1) of Pixi.js.

<!--more-->

Nothing really major but here are the changes:

* All `MovieClip` instances are renamed as `AnimatedSprite`
* Examples are updated using the `PIXI.Application` function
* `requestAnimationFrame` instances in `MoveEngine` is now updated with `Pixi.ticker.Ticker`

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
