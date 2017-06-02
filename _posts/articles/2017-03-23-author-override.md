---
layout: post
title: "Override Author Byline Test Post"
excerpt: "An article to test overriding the default site author."
categories: articles
tags: [sample-post, readability]
author: Christos Sakellariou
comments: true
share: true
modified: 2016-06-01T14:18:57-04:00
image:
  feature: so-simple-sample-image-7.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---

For those of you who may have content written by multiple authors on your site you can now assign different authors to each post if desired.

Previously the theme used a global author for the entire site and those attributes would be used in all bylines, social networking links, Twitter Card attribution, and Google Authorship. These `owner` variables were defined in `config.yml`

Start by modifying or creating a new `authors.yml` file in the `_data` folder and add your authors using the following format.

```yaml
# Authors

rocky_balboa:
  name: Rocky Balboa
  email: rocky@balboa.com
  bio: "No pain!"
  avatar: bio-photo.jpg
  twitter: rockybalboa
  google:
    plus: rockybalboa

appolo_creed:
  name: Apollo Creed
  web: http://apollocreed.com
  email: apollo@creed.com
  bio: "The Master of Disaster"
  avatar: bio-photo-2.jpg
  twitter: apollocreed
  google:
    plus: apollocreed

```

To assign Rocky Balboa as an author for our post. You'd add the following YAML front matter to a post:

```yaml
author: rocky_balboa
```
