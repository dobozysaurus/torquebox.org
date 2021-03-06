---
title: TorqueBox 1.0.0.Beta14 Released
author: Bob McWhirter
version: 1.0.0.Beta14
layout: release
tags: [ releases ]
---
After a hiatus while I worked on cloud stuff, we're back to cranking on TorqueBox.

I'm happy to announce version 1.0.0.Beta14. 

[Download](/download) it and [read the documentation](/documentation/#{page.version}/).

This release includes only a few changes:

* Fewer gratuitous JRuby runtime instantiations. This shaved ~30s off application deployment time on my laptop
* Standardized on YAML-handling library.  This reduces our dependencies by 1.
* Support for public/index.html being served if necessary.
* Re-enabling symlink deployments of RAILS_ROOTs
* Documentation improvements by Jean Deruelle on the telecom/SIP bits

For this release, Rails 2.3.2 is still the recommended version to use.  Rails 2.3.3 will work, but controllers will not be reloaded, even in the development environment. 

Rails 2.3.4 fails entirely.  It appears that in fixing the controller-reloading bug in 2.3.3, they possibly added a thread/mutex bug.

So, stick with 2.3.2, or 2.3.3 if you don't mind not having your code reload.

The next release will focus on getting us working happily with the latest versions of Rails.

