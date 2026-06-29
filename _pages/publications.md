---
layout: page
permalink: /publications/
title: publications
description: Publications in reverse chronological order.
nav: true
nav_order: 4
---

{% include bib_search.liquid %}

<div class="publications">

{% bibliography --query @*[category!=editorial][category!=other] %}

</div>

## Editorials and Other Publications

<div class="publications">

{% bibliography --query @*[category=editorial] %}

{% bibliography --query @*[category=other] %}

</div>
