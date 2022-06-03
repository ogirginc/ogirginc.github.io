---
title: How to deploy Jekyll through Github Actions
layout: post
date: '2022-06-03 11:35:00 +0300'
permalink: "/en/deploy-jekyll-through-github-actions"
categories:
- jekyll
- github pages
- github actions
---

{% if page.updated %}
  {% assign updated_ago = page.updated | timeago %}
  <details>
      <summary>
        <small><em>Last updated at {{ updated_ago }}.</em></small>
      </summary>
      <small>1. <mark>{{ updated_ago | capitalize }}</mark> – <em>Add "last updated" section to articles.</em></small>
  </details>
  <p></p>
{% endif %}

If you host a Jekyll site on Github Pages, you are probably aware of the restrictions; only gems allowed by Github can be used. [Here](https://pages.github.com/versions/) is the list, if you want to check.

However, if you want far more control over the build and still want to host your Jekyll site on GitHub Pages, you can use GitHub Actions.
