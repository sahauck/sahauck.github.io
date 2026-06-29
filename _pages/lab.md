---
layout: page
title: planetary geodynamics lab
permalink: /lab/
description:
nav: true
nav_order: 3
---

The Planetary Geodynamics group focuses on the processes the govern the evolution of planets, especially their interiors.  We examine problems that range from the crystallization of metallic cores to how crusts and mantles co-evolve to the stories embedded in the variations in the crust and lithosphere, and more.

We approach these problems with a combination of analyzing data from spacecraft missions to numerical modeling and theory.  The group has access to computing power in the lab and as part of Case Western Reserve University's High Performance Computing Center. We have been involved in multiple spacecraft missions, most notably NASA's MESSENGER mission to Mercury.  Participation in, and development of, spacecraft missions to explore our solar system is a primary activity and goal in the research group.

Teamwork + Science is what we are about here.  The people we work with are the real primary focus, with planetary science and geophysics the avenue for exploring the solar system together.

---

## Regular Collaborators

- [James Van Orman — Experimental Geochemistry](https://casfaculty.case.edu/james-van-orman/), Case Western Reserve University — Our groups collaborate on understanding deep planetary interiors, primarily planetary cores and deep mantles.  
- [Andrew Dombard — Planetary Geophysics](https://eaes.uic.edu/profiles/dombard-andrew-j/), University of Illinois Chicago — We tend to collaborate on problems of planetary tectonics and geophysics, finite element modeling, exploring the planets. 
- [Catherine L. Johnson — Planetary Geophysics](https://cljohnson.eoas.ubc.ca/), University of British Columbia and Planetary Science Institute — We primarily collaborate around planetary geophysics and exploration problems, most often Mercury.

---

## Current Members

{% assign current_members = site.data.people | where: "current", true %}
{% assign current_role_groups = "Postdoctoral Associate,Graduate Student,Undergraduate Researcher" | split: "," %}

{% for role in current_role_groups %}
  {% assign group = current_members | where: "role", role %}
  {% if group.size > 0 %}
### {{ role }}s

{% for person in group %}- {% if person.url %}[**{{ person.name }}**]({{ person.url }}){% else %}**{{ person.name }}**{% endif %}{% if person.topic %} — {{ person.topic }}{% endif %}
{% endfor %}
  {% endif %}
{% endfor %}

---

## Alumni

{% assign alumni = site.data.people | where: "current", false %}
{% assign alumni_role_groups = "Postdoctoral Associate,Graduate Student,Undergraduate Researcher" | split: "," %}

{% for role in alumni_role_groups %}
  {% assign group = alumni | where: "role", role %}
  {% if group.size > 0 %}
### {{ role }}s

{% for person in group %}- {% if person.url %}[**{{ person.name }}**]({{ person.url }}){% else %}**{{ person.name }}**{% endif %}{% if person.topic %} — {{ person.topic }}{% endif %}{% if person.position %}. Recent position: *{{ person.position }}*{% endif %}
{% endfor %}
  {% endif %}
{% endfor %}
