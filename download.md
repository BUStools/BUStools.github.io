---
layout: page
title: "Download"
group: navigation
---

{% include JB/setup %}


#### Releases

The __bustools__ GitHub repository is [here](https://github.com/BUStools/bustools/).

Binaries for Linux, Mac and Windows can be downloaded via the links below:

<table class="table">
  <thead>
    <tr>
      <th style="text-align: left">Version</th>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>

{% for post in site.categories.releases %}
    <tr>
    	<td>Release notes: <a href="{{ site.url }}/bustools_site/{{ post.url }}">{{ post.version }}</a></td>
    	<td><span class="entry-date"><time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%B %d, %Y" }}</time></span></td>

        <td><a href="https://github.com/BUStools/bustools/releases/download/{{ post.version }}/bustools_mac-{{ post.version }}.tar.gz">Mac</a></td>
        <td><a href="https://github.com/BUStools/bustools/releases/download/{{ post.version }}/bustools_linux-{{ post.version }}.tar.gz">Linux</a>  </td>
        <td><a href="https://github.com/BUStools/bustools/releases/download/{{ post.version }}/bustools_windows-{{ post.version }}.zip">Windows</a> </td>
        
        <td><a href="https://github.com/BUStools/bustools/archive/{{ post.version }}.tar.gz">Source</a></td>
    </tr>
{% endfor %}
</table>


#### Licence

__bustools__ is distributed under the BSD 2-clause "Simplified" License:

~~~
BSD 2-Clause License

Copyright (c) 2018, BUStools
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this
  list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice,
  this list of conditions and the following disclaimer in the documentation
  and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
~~~
