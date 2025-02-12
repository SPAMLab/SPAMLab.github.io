---
layout: page
title: Tags
permalink: /tags/
hero_image: "/img/headers/header_books.jpg"
hero_darken: true
show_sidebar: false
---
{%- capture site_tags -%}
    {%- for tag in site.tags -%}
        {{- tag | first -}}{%- unless forloop.last -%},{%- endunless -%}
    {%- endfor -%}
{%- endcapture -%}
{%- assign tags_list = site_tags | split:',' | sort -%}


### All tags  

<div class="buttons">
{%- for tag in tags_list -%}
    <button class="button is-info is-light is-link">
        <a href="#{{- tag -}}">&nbsp;{{- tag -}}&nbsp;({{site.tags[tag].size}})</a>
    </button>
{%- endfor -%}
</div>

<br>
<br>

### Posts 

<div class="content">
{%- for tag in tags_list -%}
    <h2 id="{{- tag -}}">
        <button class="button is-info is-medium is-light is-static">&nbsp;{{- tag -}}&nbsp;({{site.tags[tag].size}})</button>
    </h2>
    <div>
        {%- for post in site.tags[tag] -%}
            <div>
                <a href="{{- site.baseurl -}}{{- post.url -}}">{{- post.title -}}</a>
            </div>
            <br>
        {%- endfor -%}
        <button class="button is-small"><span class="icon is-small"><i class="fa-solid fa-arrow-up"></i></span><span><a href="#top"> back to top</a></span></button>
    </div>
    <br>
{%- endfor -%}
</div>
