---
layout: master
title: Websanova Search
keywords: websanova, search
description: Search Websanova (powered by Google)
permalink: /search.html
disqus: false
---

<style>
    form {position: relative; width: 100%; padding-right: 110px;}
    #search input { border: solid #cacaca 1px;}
    #search button {position:absolute; right: 0px; top: 0px; background-color: #428bca; width: 100px;}
    #search input,
    #search button {
        height: 30px;
    }
</style>

<form id="search" action="/search">
    <input name="q" type="text" placeholder="search" />
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
<gcse:searchresults-only></gcse:searchresults-only>