---
title: Releasing a Release
layout: default
---

[release-repo]: http://github.com/torquebox/torquebox-release

# Preparation

Code is released from the `master` branch of the [torquebox/torquebox-release][release-repo].

Set up this repository as an additional remote for your workspace:

    git remote add release git@github.com:torquebox/torquebox-release.git

Ensure that the tag you are attempting to release does not exist in the release repository,
or maven will fail part way through the build

    git push release :1.0.2

Ensure that the `master` branch has the contents you wish to release.  Using the `-f`
flag to force is allowed in this case, since the **torquebox-release** repository is not
a public-facing human-cloneable repository.

    git push release master -f

# Pre-flight build

Using the [build system](http://torquebox.ci.cloudbees.com/), select the 
**torquebox-release** job.

<img src="/images/releasing/ci.png" style="width: 100%"/>

Enter in the version to release, followed by the **next** version after the release, and
select the **local** profile.

<img src="/images/releasing/start-preflight.png" style="width: 100%"/>

After each pre-flight build, you will need to reset the release repository:

    git push release :1.0.2
    git push release master -f
    
When you are happy with the pre-flight build (in other words, it completes successfully), 
you're ready to run the real build.

# Perform the Builds

Using the [build system](http://torquebox.ci.cloudbees.com/), again select the 
**torquebox-release** job, selecting the **bees** profile this time.

<img src="/images/releasing/start-build.png" style="width: 100%"/>

# Verify the maven artifacts

Verify that the artifacts you expect have been uploaded and deployed to

[http://repository-torquebox.forge.cloudbees.com/release](http://repository-torquebox.forge.cloudbees.com/release)

# Manually deploy RubyGems

Once the build has completed, grab the gems from 
[https://torquebox.ci.cloudbees.com//job/torquebox-release/lastSuccessfulBuild/artifact/assemblage/assembly/target/stage/gem-repo/gems/](https://torquebox.ci.cloudbees.com//job/torquebox-release/lastSuccessfulBuild/artifact/assemblage/assembly/target/stage/gem-repo/gems/}}) using the 'all files as zip' link push each gem to rubygems.org (you'll need owner rights to do so - bug bobmcw or tcrawley):

    gem push <gem_name>.gem
    
Since rubygems checks that the dependencies are available on push, you'll have to push them in a proper order. The
order that worked for the 1.0.1 release was:

* torquebox-vfs
* torquebox-base
* torquebox-capistrano-support
* torquebox-rake-support
* torquebox-container-foundation
* torquebox-messaging-container
* torquebox-naming-container
* torquebox-naming
* torquebox-messaging
* torquebox-web
* torquebox

# Push changes from the release repository to the official repository

## Fetch the release commits locally:

    git fetch release

## Merge in the release commits:

    git merge release/master

## Push to the official repository

    git push origin master

## Push the tag to the official repository

    git push origin 1.0.2

# Release the project in JIRA

<img src="/images/releasing/jira-release.png" style="width: 100%"/>

# Announce it

## Post it on `torquebox.org`

## Notify the `torquebox-users@` list

## Tweet it.