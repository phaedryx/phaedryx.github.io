---
layout: post
title:  "Choosing a Rails CMS"
date:   2015-04-20
summary: My team looked through a variety of CMS's and ultimately chose the Alchemy CMS
categories: ruby rails cms engine
---

I recently started a new job as the web team lead on the for a
marketing firm. The entire team is fairly new to the company so we are making
decisions about what workflows to develop and which tools to use. We decided
that in order to keep up with requirements, we would need to leverage existing
software instead of creating every website from scratch. For us, that meant
finding a good CMS that met our needs.

We established the following criteria:

  1. It must be open source (code we can see and modify)
  2. It must be up-to-date (currently Ruby on Rails 4 and Ruby 2)
  3. It must be flexible (e.g. mountable as an engine or used for an entire site)
  4. It must be intuitive for non-developers to admin
  5. It must be supported by a company who's needs reflect our own.

The last point is especially important because we didn't want to be working at
cross-purposes. For example, if a company behind a CMS made most of their money
from hosting their CMS, they would have no motivation to make their CMS easy to
configure and install; in fact the opposite is true. We, however, need to be
able to quickly get our websites set up.

We made a list of candidates based on searches and the [CMS list on the Ruby Toolbox](https://www.ruby-toolbox.com/categories/content_management_systems).
[Radiant](https://github.com/radiant/radiant/) looked promising, but we found it
woefully outdated (Rails 2.3). [Locomotive](https://github.com/locomotivecms/engine)
was interesting, but had a lot of setup overhead.
[Comfortable Mexican Sofa](https://github.com/comfy/comfortable-mexican-sofa) had
a lot of the things we were looking for, but, ultimately,
[Alchemy](http://alchemy-cms.com/about) was the only one that met all of our
criteria.

Alchemy is [open source](https://github.com/AlchemyCMS/alchemy_cms), is up-to-date,
is mountable as an engine, is intuitive for our users, and is backed by a company,
[Magic Labs](https://magiclabs.de/en/home) that has needs similar to our own.
The most apparent drawback is the modest documentation, but [the code](http://www.rubydoc.info/github/magiclabs/alchemy_cms/index) has decent comments.
Overall, it is a good fit for us.

Lastly, I decided to test the waters by [submitting a pull request](https://github.com/AlchemyCMS/alchemy_cms/pull/760). They were friendly
and supportive, but requested some tests before they merged it. Perfect.
