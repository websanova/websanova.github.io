---
layout: master
title: JavaScript RGB to Hex Converter
keywords: javascript, rgb, hex, converter
description: An article with sample code for a smiple JavaScript RGB to HEX converter.
date: Mar 28 2012
permalink: /blog/javascript/javascript-rgb-to-hex-converter.html
redirect_from:
  - /tutorials/javascript/javascript-rgb-to-hex-converter.html
  - /javascript-rgb-to-hex-converter.html
---

This is a little script for converting between RGB and Hex color codes in JavaScript.  I have broken it down into three pieces for converting from RGB to Hex, Hex to RGB and finally a combination of the two that will auto detect the correct color code coming in and convert it to the appropriate one.  The scripts also make sure the color codes are valid and if an invalid color code is passed into the functions the script will return the value false.

### Hex to RGB

Here we accept both shorthand and full hex codes in the form "#AAA" or "#CACACA" respectively.  It makes sure the hex code begins with "#" and only contains the characters 0-9 and A-F.  It then takes each RGB Hex value and converts them into RGB values and returns a string in the format "rgb(25, 255, 67)".

~~~
function hexToRgb(string)
{
    if(!string || typeof string !== 'string') return false;

    if(
        string.substring(0,1) == '#' &amp;&amp; 
        (string.length == 4 || string.length == 7) &amp;&amp; 
        /^[0-9a-fA-F]+$/.test(string.substring(1, string.length))
    ){
        string = string.substring(1, string.length);
        
        if(string.length == 3) 
            string = string[0] + string[0] + string[1] + string[1] + string[2] + string[2];
        
        return 'rgb(' + 
            parseInt(string[0] + string[1], 16).toString() + ',' + 
            parseInt(string[2] + string[3], 16).toString() + ',' + 
            parseInt(string[4] + string[5], 16).toString() + 
        ')';
    }
    else return false;
}
~~~

### RGB to Hex

For the RGB conversion we need to do a bit of parsing first.  We start by making sure the string starts with `rgb`, then we make sure it has three values between 0-255.  After that we simply convert them to base16 strings and return a hex string in the format "#4ACA79".

~~~
function rgbToHex(string)
{
    if(!string || typeof string !== 'string') return false;
    
    if(string.substring(0,3) == 'rgb')
    {
        string = string.substring(4, string.length - 1).split(',');
        
        if(string.length == 3 &amp;&amp; string[0] != '' &amp;&amp; string[1] != '' &amp;&amp; string[2] != '')
        {
            if(
                string[0] &gt;= 0 &amp;&amp; string[0] &lt;= 255 &amp;&amp; 
                string[1] &gt;= 0 &amp;&amp; string[1] &lt;= 255 &amp;&amp; 
                string[2] &gt;= 0 &amp;&amp; string[2] &lt;= 255
            ){
                return ('#' + 
                    parseInt(string[0]).toString(16) + 
                    parseInt(string[1]).toString(16) + 
                    parseInt(string[2]).toString(16)
                ).toUpperCase();
            }
            else return false;
        }
        else return false;
    }
    else return false;
}
~~~

### Putting it Together

This is just a combination of the two scripts above.  We will auto detect the input coming in for either `rgb` or `#` and then do the appropriate conversion.

~~~
function hexOrRgb(string)
{
    if(!string || typeof string !== 'string') return false;
    
    if(
        string.substring(0,1) == '#' &amp;&amp; 
        (string.length == 4 || string.length == 7) &amp;&amp; 
        /^[0-9a-fA-F]+$/.test(string.substring(1, string.length))
    ){
        string = string.substring(1, string.length);
        
        if(string.length == 3) 
            string = string[0] + string[0] + string[1] + string[1] + string[2] + string[2];
        
        return 'rgb(' + 
            parseInt(string[0] + string[1], 16).toString() + ',' + 
            parseInt(string[2] + string[3], 16).toString() + ',' + 
            parseInt(string[4] + string[5], 16).toString() + 
        ')';
    }
    else if(string.substring(0,3) == 'rgb')
    {
        string = string.substring(4, string.length - 1).split(',');
        
        if(string.length == 3 &amp;&amp; string[0] != '' &amp;&amp; string[1] != '' &amp;&amp; string[2] != '')
        {
            if(
                string[0] &gt;= 0 &amp;&amp; string[0] &lt;= 255 &amp;&amp; 
                string[1] &gt;= 0 &amp;&amp; string[1] &lt;= 255 &amp;&amp; 
                string[2] &gt;= 0 &amp;&amp; string[2] &lt;= 255
            ){
                return ('#' + 
                    parseInt(string[0]).toString(16) + 
                    parseInt(string[1]).toString(16) + 
                    parseInt(string[2]).toString(16)
                ).toUpperCase();
            }
            else return false;
        }
        else return false;
    }
    else return false;
}
~~~