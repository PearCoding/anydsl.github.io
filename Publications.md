---
title: Publications
weight: 10
parent: index.md
---

{% if site.data.publications.papers %}
{% assign papers = site.data.publications.papers | sort: 'date' | reverse %}
{% if papers.size > 0 %}
## Conference Papers

{% for pub in papers %}
{{ pub.authors | array_to_sentence_string: 'and' }} <br/> *[{{ pub.title }}]({{ pub.url }})* <br/> {% if pub.awarding %}**({{ pub.awarding }})** <br/>{% endif %} {{ pub.venue }}

{% endfor %}
{% endif %}
{% endif %}

{% if site.data.publications.posters %}
{% assign posters = site.data.publications.posters | sort: 'date' | reverse %}
{% if posters.size > 0 %}
## Posters

{% for pub in posters %}
{{ pub.authors | array_to_sentence_string: 'and' }} <br/> *[{{ pub.title }}]({{ pub.url }})* <br/> {% if pub.awarding %}**({{ pub.awarding }})** <br/>{% endif %} {{ pub.venue }}

{% endfor %}
{% endif %}
{% endif %}

{% if site.data.publications.misc %}
{% assign misc = site.data.publications.misc | sort: 'date' | reverse %}
{% if misc.size > 0 %}
## Misc.

{% for pub in misc %}
{{ pub.authors | array_to_sentence_string: 'and' }} <br/> *[{{ pub.title }}]({{ pub.url }})* <br/> {% if pub.awarding %}**({{ pub.awarding }})** <br/>{% endif %} {{ pub.venue }}

{% endfor %}
{% endif %}
{% endif %}
