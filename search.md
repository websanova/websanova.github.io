---
layout: master
title: Websanova Search
keywords: websanova, search
description: Search Websanova (powered by Google)
permalink: /search.html
page: true
---

<style>
    form {position: relative; width: 100%; padding-right: 100px;}
    #search input { border: solid #cacaca 1px; width: 100%; padding: 0 5px;}
    #search button {position:absolute; right: 0px; top: 0px; border:none; color: #fafafa; background-color: #428bca; width: 90px;}
    #search input, #search button {height: 30px;}
</style>

<div style="height: 25px;">&nbsp;</div>

<form id="search" action="/search">
    <input name="q" type="text" placeholder="Search Websanova" />
    <button type="submit">Search</button>
</form>

<script>
  (function() {
    var cx = '017644839775759296827:rpwkqyb7abs';
    var gcse = document.createElement('script');
    gcse.type = 'text/javascript';
    gcse.async = true;
    gcse.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') +
        '//cse.google.com/cse.js?cx=' + cx;
    var s = document.getElementsByTagName('script')[0];
    s.parentNode.insertBefore(gcse, s);
  })();
</script>
<gcse:searchresults-only linktarget="_parent"></gcse:searchresults-only>