---
layout: master
title: wPaint 2.0 Released!
keywords: wpaint, jquery, plugins, websanova, paint, javascript
description: It’s been long overdue but wPaint 2.0 is finally out. The 1.0 stream was doing well but I was getting continually emailed with feature requests, and bugs particularly on mobile. With this release wPaint has been completely rewritten from the ground up. 
date: Sep 03 2013
permalink: /blog/jquery/wpaint-2-0-released.html
---

It’s been long overdue but wPaint 2.0 is finally out. The 1.0 stream was doing well but I was getting continually emailed with feature requests, and bugs particularly on mobile. With this release wPaint has been completely rewritten from the ground up. New things like multiple themeing and the ability to add plugins/modules to wPaint have been added to make it much easier to build upon and extend. This makes it both easier for me to maintain and continue to grow with new feature requests as well as making it easier for other developers to add their own plugins to wPaint.

* [wPaint Demo](http://wpaint.websanova.com/)
* [wPaint Download](https://github.com/websanova/wPaint/tags)
* [wPaint Documentation](https://github.com/websanova/wPaint#wpaintjs)
* [wPaint Issues](https://github.com/websanova/wPaint/issues)

Below is a list of changes for wPaint 2.0:

* Extensions are probably the most exciting feature is the ability to extend wPaint in a plugin type format. This allows you to add extra menus, buttons/icons, cursors, and defaults in a standalone modular way.
* Mobile has been cleaned up a lot of the mobile code and have tested more thoroughly on a variety of devices. This should fix a lot of the bugs that were being reported by Android users.
* Groups of icons can now be stacked and put into a drop down so common sets of shapes can be grouped together without taking up space on the menu.
* Drawing functions are much more separated now, each function and drawing is it’s own independent unit with an `up`, `move` and `down` component.
* More efficient algorithm for bucket (fill area) tool, thanks to [William Malone](http://www.williammalone.com/articles/html5-canvas-javascript-paint-bucket-tool/).
* Dropper icon added to [wColorPicker](http://wcolorpicker.websanova.com/) allowing you to select colors separately for each palette.
* Menu handle is now toggleable which means you can deactivate dragging.
Added cursor images for each icon with different sizes for eraser tool.
* Custom `html` based drop downs which make for much cleaner and consistent looking drop downs.
* Allow multiple themes such as `classic` and `standard` for style and size respectively.
* Both background and image can now be set to either a base64 encoded png, and image path or a valid RGB or Hex color.
* Auto scaling for images to fit the canvas fully and auto centering for image and background have been added.
* Modal function helper has been added allowing you to trigger a modal, for example in the load image button
* Status function helper also added as a status message that can be used in thins such as saving the wPaint image.
* Documentation has been greatly improved.