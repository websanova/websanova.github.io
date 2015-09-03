---
layout: master
title: 10 Shapes to Extend HTML5 Canvas
keywords: html5, canvas, shapes, draw, paint, wpaint
description: Today we extended HTML5 Canvas to easily allow drawing some shapes through the use of simple methods.
date: Aug 14 2013
permalink: /blog/html5/10-shapes-to-extend-html5-canvas.html
---

Today I wanted to take a look at extending the HTML5 canvas object to add shapes to it. The canvas object comes with some basic functions allowing us to draw things like rectangles and lines but it doesn’t come packaged with even a more basic subset of shapes like circles, ellipses or rounded corners. I was creating a variety of shapes for the new [wPaint 2.0](http://wpaint.websanova.com/) version I’m currently working on and thought I would share some of the functions I created.

The idea is to streamline the canvas object so that we can just make function calls like `.rect(x, y, w, h)`, `.ellipse(x, y, w, h)`, `.roundedRec(x, y, w, h, radius)` and so on. Below are 10 shape examples with a couple bonuses like `fillArea` and `ninjaStar`.

Also feel free to check out the [GitHub page](https://github.com/websanova/wExtensions) where you can add your own shapes or create an issue for suggestions or fixes for new or existing shapes.

<script type="text/javascript">
CanvasRenderingContext2D.prototype.diamond=function(a,b,c,d){return a&&b&&c&&d?(this.beginPath(),this.moveTo(a+.5*c,b),this.lineTo(a,b+.5*d),this.lineTo(a+.5*c,b+d),this.lineTo(a+c,b+.5*d),this.lineTo(a+.5*c,b),this.closePath(),void 0):!0},CanvasRenderingContext2D.prototype.diamond=function(a,b,c,d){return a&&b&&c&&d?(this.beginPath(),this.moveTo(a+.5*c,b),this.lineTo(a,b+.5*d),this.lineTo(a+.5*c,b+d),this.lineTo(a+c,b+.5*d),this.lineTo(a+.5*c,b),this.closePath(),void 0):!0},CanvasRenderingContext2D.prototype.diamond=function(a,b,c,d){return a&&b&&c&&d?(this.beginPath(),this.moveTo(a+.5*c,b),this.lineTo(a,b+.5*d),this.lineTo(a+.5*c,b+d),this.lineTo(a+c,b+.5*d),this.lineTo(a+.5*c,b),this.closePath(),void 0):!0},CanvasRenderingContext2D.prototype.ellipse=function(a,b,c,d){if(!(a&&b&&c&&d))return!0;var e=.5522848,f=c/2*e,g=d/2*e,h=a+c,i=b+d,j=a+c/2,k=b+d/2;this.beginPath(),this.moveTo(a,k),this.bezierCurveTo(a,k-g,j-f,b,j,b),this.bezierCurveTo(j+f,b,h,k-g,h,k),this.bezierCurveTo(h,k+g,j+f,i,j,i),this.bezierCurveTo(j-f,i,a,k+g,a,k),this.closePath()},CanvasRenderingContext2D.prototype.fillArea=function(a,b,c){function d(a){return{r:o[a],g:o[a+1],b:o[a+2],a:o[a+3]}}function e(a){o[a]=c[0],o[a+1]=c[1],o[a+2]=c[2],o[a+3]=c[3]}function f(a){return p.r===a.r&&p.g===a.g&&p.b===a.b&&p.a===a.a}if(!a||!b||!c)return!0;var g=this.canvas.style.color;this.canvas.style.color=c,c=this.canvas.style.color.match(/^rgba?\((.*)\);?$/)[1].split(","),this.canvas.style.color=g,c[0]=parseInt(c[0],10),c[1]=parseInt(c[1],10),c[2]=parseInt(c[2],10),c[3]=parseInt(c[3]||255,10);for(var h,i,j,k,l=this.canvas.width,m=this.canvas.height,n=this.getImageData(0,0,l,m),o=n.data,p=d(4*(b*l+a)),q=[[a,b]];q.length;){for(h=q.pop(),i=4*(h[1]*l+h[0]);h[1]-->=0&&f(d(i));)i-=4*l;for(i+=4*l,++h[1],j=!1,k=!1;h[1]++<m-1&&f(d(i));)e(i),h[0]>0&&(f(d(i-4))?j||(q.push([h[0]-1,h[1]]),j=!0):j&&(j=!1)),h[0]<l-1&&(f(d(i+4))?k||(q.push([h[0]+1,h[1]]),k=!0):k&&(k=!1)),i+=4*l}this.putImageData(n,0,0)},CanvasRenderingContext2D.prototype.hexagon=function(a,b,c,d){if(!(a&&b&&c&&d))return!0;var e=.225,f=1-e;this.beginPath(),this.moveTo(a+.5*c,b),this.lineTo(a,b+d*e),this.lineTo(a,b+d*f),this.lineTo(a+.5*c,b+d),this.lineTo(a+c,b+d*f),this.lineTo(a+c,b+d*e),this.lineTo(a+.5*c,b),this.closePath()},CanvasRenderingContext2D.prototype.ninjaStar=function(a,b,c,d){return a&&b&&c&&d?(this.beginPath(),this.moveTo(a+.5*c,b),this.lineTo(a+.35*c,b+.35*d),this.lineTo(a,b+.5*d),this.lineTo(a+.35*c,b+.65*d),this.lineTo(a+.5*c,b+d),this.lineTo(a+.65*c,b+.65*d),this.lineTo(a+c,b+.5*d),this.lineTo(a+.65*c,b+.35*d),this.lineTo(a+.5*c,b),this.closePath(),void 0):!0},CanvasRenderingContext2D.prototype.octagon=function(a,b,c,d){if(!(a&&b&&c&&d))return!0;var e=.275,f=1-e;this.beginPath(),this.moveTo(a+c*e,b),this.lineTo(a,b+d*e),this.lineTo(a,b+d*f),this.lineTo(a+c*e,b+d),this.lineTo(a+c*f,b+d),this.lineTo(a+c,b+d*f),this.lineTo(a+c,b+d*e),this.lineTo(a+c*f,b),this.lineTo(a+c*e,b),this.closePath()},CanvasRenderingContext2D.prototype.parallelogram=function(a,b,c,d){return a&&b&&c&&d?(this.beginPath(),this.moveTo(a+.3*c,b),this.lineTo(a,b+d),this.lineTo(a+.7*c,b+d),this.lineTo(a+c,b),this.lineTo(a+.3*c,b),this.closePath(),void 0):!0},CanvasRenderingContext2D.prototype.pentagon=function(a,b,c,d){return a&&b&&c&&d?(this.beginPath(),this.moveTo(a+c/2,b),this.lineTo(a,b+.4*d),this.lineTo(a+.2*c,b+d),this.lineTo(a+.8*c,b+d),this.lineTo(a+c,b+.4*d),this.lineTo(a+c/2,b),this.closePath(),void 0):!0},CanvasRenderingContext2D.prototype.roundedRect=function(a,b,c,d,e){return a&&b&&c&&d?(e||(e=5),this.beginPath(),this.moveTo(a+e,b),this.lineTo(a+c-e,b),this.quadraticCurveTo(a+c,b,a+c,b+e),this.lineTo(a+c,b+d-e),this.quadraticCurveTo(a+c,b+d,a+c-e,b+d),this.lineTo(a+e,b+d),this.quadraticCurveTo(a,b+d,a,b+d-e),this.lineTo(a,b+e),this.quadraticCurveTo(a,b,a+e,b),this.closePath(),void 0):!0},CanvasRenderingContext2D.prototype.star=function(a,b,c,d){return a&&b&&c&&d?(this.beginPath(),this.moveTo(a+.5*c,b),this.lineTo(a+.3*c,b+.3*d),this.lineTo(a,b+.4*d),this.lineTo(a+.2*c,b+.65*d),this.lineTo(a+.2*c,b+d),this.lineTo(a+.5*c,b+.8*d),this.lineTo(a+.8*c,b+d),this.lineTo(a+.8*c,b+.65*d),this.lineTo(a+c,b+.4*d),this.lineTo(a+.7*c,b+.3*d),this.lineTo(a+.5*c,b),this.closePath(),void 0):!0},CanvasRenderingContext2D.prototype.star=function(a,b,c,d){return a&&b&&c&&d?(this.beginPath(),this.moveTo(a+.5*c,b),this.lineTo(a+.375*c,b+.4*d),this.lineTo(a,b+.4*d),this.lineTo(a+.3*c,b+.625*d),this.lineTo(a+.2*c,b+d),this.lineTo(a+.5*c,b+.725*d),this.lineTo(a+.8*c,b+d),this.lineTo(a+.7*c,b+.625*d),this.lineTo(a+c,b+.4*d),this.lineTo(a+.625*c,b+.4*d),this.lineTo(a+.5*c,b),this.closePath(),void 0):!0},CanvasRenderingContext2D.prototype.trapezoid=function(a,b,c,d){return a&&b&&c&&d?(this.beginPath(),this.moveTo(a+.2*c,b),this.lineTo(a,b+d),this.lineTo(a+c,b+d),this.lineTo(a+.8*c,b),this.lineTo(a+.3*c,b),this.closePath(),void 0):!0},CanvasRenderingContext2D.prototype.triangle=function(a,b,c,d){return a&&b&&c&&d?(this.beginPath(),this.moveTo(a+c/2,b),this.lineTo(a,b+d),this.lineTo(a+c,b+d),this.lineTo(a+c/2,b),this.closePath(),void 0):!0},CanvasRenderingContext2D.prototype.ellipse=function(a,b,c,d){if(!(a&&b&&c&&d))return!0;var e=.5522848,f=c/2*e,g=d/2*e,h=a+c,i=b+d,j=a+c/2,k=b+d/2;this.beginPath(),this.moveTo(a,k),this.bezierCurveTo(a,k-g,j-f,b,j,b),this.bezierCurveTo(j+f,b,h,k-g,h,k),this.bezierCurveTo(h,k+g,j+f,i,j,i),this.bezierCurveTo(j-f,i,a,k+g,a,k),this.closePath()},CanvasRenderingContext2D.prototype.fillArea=function(a,b,c){function d(a){return{r:o[a],g:o[a+1],b:o[a+2],a:o[a+3]}}function e(a){o[a]=c[0],o[a+1]=c[1],o[a+2]=c[2],o[a+3]=c[3]}function f(a){return p.r===a.r&&p.g===a.g&&p.b===a.b&&p.a===a.a}if(!a||!b||!c)return!0;var g=this.canvas.style.color;this.canvas.style.color=c,c=this.canvas.style.color.match(/^rgba?\((.*)\);?$/)[1].split(","),this.canvas.style.color=g,c[0]=parseInt(c[0],10),c[1]=parseInt(c[1],10),c[2]=parseInt(c[2],10),c[3]=parseInt(c[3]||255,10);for(var h,i,j,k,l=this.canvas.width,m=this.canvas.height,n=this.getImageData(0,0,l,m),o=n.data,p=d(4*(b*l+a)),q=[[a,b]];q.length;){for(h=q.pop(),i=4*(h[1]*l+h[0]);h[1]-->=0&&f(d(i));)i-=4*l;for(i+=4*l,++h[1],j=!1,k=!1;h[1]++<m-1&&f(d(i));)e(i),h[0]>0&&(f(d(i-4))?j||(q.push([h[0]-1,h[1]]),j=!0):j&&(j=!1)),h[0]<l-1&&(f(d(i+4))?k||(q.push([h[0]+1,h[1]]),k=!0):k&&(k=!1)),i+=4*l}this.putImageData(n,0,0)},CanvasRenderingContext2D.prototype.hexagon=function(a,b,c,d){if(!(a&&b&&c&&d))return!0;var e=.225,f=1-e;this.beginPath(),this.moveTo(a+.5*c,b),this.lineTo(a,b+d*e),this.lineTo(a,b+d*f),this.lineTo(a+.5*c,b+d),this.lineTo(a+c,b+d*f),this.lineTo(a+c,b+d*e),this.lineTo(a+.5*c,b),this.closePath()},CanvasRenderingContext2D.prototype.ninjastar=function(a,b,c,d){return a&&b&&c&&d?(this.beginPath(),this.moveTo(a+.5*c,b),this.lineTo(a+.35*c,b+.35*d),this.lineTo(a,b+.5*d),this.lineTo(a+.35*c,b+.65*d),this.lineTo(a+.5*c,b+d),this.lineTo(a+.65*c,b+.65*d),this.lineTo(a+c,b+.5*d),this.lineTo(a+.65*c,b+.35*d),this.lineTo(a+.5*c,b),this.closePath(),void 0):!0},CanvasRenderingContext2D.prototype.octagon=function(a,b,c,d){if(!(a&&b&&c&&d))return!0;var e=.275,f=1-e;this.beginPath(),this.moveTo(a+c*e,b),this.lineTo(a,b+d*e),this.lineTo(a,b+d*f),this.lineTo(a+c*e,b+d),this.lineTo(a+c*f,b+d),this.lineTo(a+c,b+d*f),this.lineTo(a+c,b+d*e),this.lineTo(a+c*f,b),this.lineTo(a+c*e,b),this.closePath()},CanvasRenderingContext2D.prototype.parallelogram=function(a,b,c,d){return a&&b&&c&&d?(this.beginPath(),this.moveTo(a+.3*c,b),this.lineTo(a,b+d),this.lineTo(a+.7*c,b+d),this.lineTo(a+c,b),this.lineTo(a+.3*c,b),this.closePath(),void 0):!0},CanvasRenderingContext2D.prototype.pentagon=function(a,b,c,d){return a&&b&&c&&d?(this.beginPath(),this.moveTo(a+c/2,b),this.lineTo(a,b+.4*d),this.lineTo(a+.2*c,b+d),this.lineTo(a+.8*c,b+d),this.lineTo(a+c,b+.4*d),this.lineTo(a+c/2,b),this.closePath(),void 0):!0},CanvasRenderingContext2D.prototype.roundedRect=function(a,b,c,d,e){return a&&b&&c&&d?(e||(e=5),this.beginPath(),this.moveTo(a+e,b),this.lineTo(a+c-e,b),this.quadraticCurveTo(a+c,b,a+c,b+e),this.lineTo(a+c,b+d-e),this.quadraticCurveTo(a+c,b+d,a+c-e,b+d),this.lineTo(a+e,b+d),this.quadraticCurveTo(a,b+d,a,b+d-e),this.lineTo(a,b+e),this.quadraticCurveTo(a,b,a+e,b),this.closePath(),void 0):!0},CanvasRenderingContext2D.prototype.star=function(a,b,c,d){return a&&b&&c&&d?(this.beginPath(),this.moveTo(a+.5*c,b),this.lineTo(a+.3*c,b+.3*d),this.lineTo(a,b+.4*d),this.lineTo(a+.2*c,b+.65*d),this.lineTo(a+.2*c,b+d),this.lineTo(a+.5*c,b+.8*d),this.lineTo(a+.8*c,b+d),this.lineTo(a+.8*c,b+.65*d),this.lineTo(a+c,b+.4*d),this.lineTo(a+.7*c,b+.3*d),this.lineTo(a+.5*c,b),this.closePath(),void 0):!0},CanvasRenderingContext2D.prototype.star=function(a,b,c,d){return a&&b&&c&&d?(this.beginPath(),this.moveTo(a+.5*c,b),this.lineTo(a+.375*c,b+.4*d),this.lineTo(a,b+.4*d),this.lineTo(a+.3*c,b+.625*d),this.lineTo(a+.2*c,b+d),this.lineTo(a+.5*c,b+.725*d),this.lineTo(a+.8*c,b+d),this.lineTo(a+.7*c,b+.625*d),this.lineTo(a+c,b+.4*d),this.lineTo(a+.625*c,b+.4*d),this.lineTo(a+.5*c,b),this.closePath(),void 0):!0},CanvasRenderingContext2D.prototype.trapezoid=function(a,b,c,d){return a&&b&&c&&d?(this.beginPath(),this.moveTo(a+.2*c,b),this.lineTo(a,b+d),this.lineTo(a+c,b+d),this.lineTo(a+.8*c,b),this.lineTo(a+.3*c,b),this.closePath(),void 0):!0},CanvasRenderingContext2D.prototype.triangle=function(a,b,c,d){return a&&b&&c&&d?(this.beginPath(),this.moveTo(a+c/2,b),this.lineTo(a,b+d),this.lineTo(a+c,b+d),this.lineTo(a+c/2,b),this.closePath(),void 0):!0};
</script>

### ctx.fillArea(x, y, color)

<canvas id="canvas-fillArea" width="100" height="100"></canvas>
<script type="text/javascript">
var ctx = document.getElementById('canvas-fillArea').getContext('2d');
ctx.fillArea(5, 5, '#6699FF');
</script>

### ctx.ellipse(x, y, w, h)

<canvas id="canvas-ellipse" width="100" height="100"></canvas>
<script type="text/javascript">
var ctx = document.getElementById('canvas-ellipse').getContext('2d');
ctx.fillStyle = '#6699ff';
ctx.strokeStyle = '#000000';
ctx.lineWidth = '2px';
ctx.ellipse(5, 5, 90, 90);
ctx.fill();
ctx.stroke();
</script>

### ctx.roundedRect(x, y, w, h, radius)

<canvas id="canvas-roundedRect" width="100" height="100"></canvas>
<script type="text/javascript">
var ctx = document.getElementById('canvas-roundedRect').getContext('2d');
ctx.fillStyle = '#6699ff';
ctx.strokeStyle = '#000000';
ctx.lineWidth = '2px';
ctx.roundedRect(5, 5, 90, 90);
ctx.fill();
ctx.stroke();
</script>

### ctx.diamond(x, y, w, h)

<canvas id="canvas-diamond" width="100" height="100"></canvas>
<script type="text/javascript">
var ctx = document.getElementById('canvas-diamond').getContext('2d');
ctx.fillStyle = '#6699ff';
ctx.strokeStyle = '#000000';
ctx.lineWidth = '2px';
ctx.diamond(5, 5, 90, 90);
ctx.fill();
ctx.stroke();
</script>

### ctx.pentagon(x, y, w, h)

<canvas id="canvas-pentagon" width="100" height="100"></canvas>
<script type="text/javascript">
var ctx = document.getElementById('canvas-pentagon').getContext('2d');
ctx.fillStyle = '#6699ff';
ctx.strokeStyle = '#000000';
ctx.lineWidth = '2px';
ctx.pentagon(5, 5, 90, 90);
ctx.fill();
ctx.stroke();
</script>

### ctx.hexagon(x, y, w, h)

<canvas id="canvas-hexagon" width="100" height="100"></canvas>
<script type="text/javascript">
var ctx = document.getElementById('canvas-hexagon').getContext('2d');
ctx.fillStyle = '#6699ff';
ctx.strokeStyle = '#000000';
ctx.lineWidth = '2px';
ctx.hexagon(5, 5, 90, 90);
ctx.fill();
ctx.stroke();
</script>

### ctx.octagon(x, y, w, h)

<canvas id="canvas-octagon" width="100" height="100"></canvas>
<script type="text/javascript">
var ctx = document.getElementById('canvas-octagon').getContext('2d');
ctx.fillStyle = '#6699ff';
ctx.strokeStyle = '#000000';
ctx.lineWidth = '2px';
ctx.octagon(5, 5, 90, 90);
ctx.fill();
ctx.stroke();
</script>

### ctx.star(x, y, w, h)

<canvas id="canvas-star" width="100" height="100"></canvas>
<script type="text/javascript">
var ctx = document.getElementById('canvas-star').getContext('2d');
ctx.fillStyle = '#6699ff';
ctx.strokeStyle = '#000000';
ctx.lineWidth = '2px';
ctx.star(5, 5, 90, 90);
ctx.fill();
ctx.stroke();
</script>

### ctx.trapezoid(x, y, w, h)

<canvas id="canvas-trapezoid" width="100" height="100"></canvas>
<script type="text/javascript">
var ctx = document.getElementById('canvas-trapezoid').getContext('2d');
ctx.fillStyle = '#6699ff';
ctx.strokeStyle = '#000000';
ctx.lineWidth = '2px';
ctx.trapezoid(5, 5, 90, 90);
ctx.fill();
ctx.stroke();
</script>

### ctx.parallelogram(x, y, w, h)

<canvas id="canvas-parallelogram" width="100" height="100"></canvas>
<script type="text/javascript">
var ctx = document.getElementById('canvas-parallelogram').getContext('2d');
ctx.fillStyle = '#6699ff';
ctx.strokeStyle = '#000000';
ctx.lineWidth = '2px';
ctx.parallelogram(5, 5, 90, 90);
ctx.fill();
ctx.stroke();
</script>

### ctx.triangle(x, y, w, h)

<canvas id="canvas-triangle" width="100" height="100"></canvas>
<script type="text/javascript">
var ctx = document.getElementById('canvas-triangle').getContext('2d');
ctx.fillStyle = '#6699ff';
ctx.strokeStyle = '#000000';
ctx.lineWidth = '2px';
ctx.triangle(5, 5, 90, 90);
ctx.fill();
ctx.stroke();
</script>

### ctx.ninjaStar(x, y, w, h)

<canvas id="canvas-ninjaStar" width="100" height="100"></canvas>
<script type="text/javascript">
var ctx = document.getElementById('canvas-ninjaStar').getContext('2d');
ctx.fillStyle = '#6699ff';
ctx.strokeStyle = '#000000';
ctx.lineWidth = '2px';
ctx.ninjaStar(5, 5, 90, 90);
ctx.fill();
ctx.stroke();
</script>