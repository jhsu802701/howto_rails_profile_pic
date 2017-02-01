# Ruby on Racetracks: Adding User Profile Pictures

## Prerequisites
* You should be comfortable using my rbenv-general Docker image. If you don't already have it set up, go to the [Ruby on Racetracks site](http://www.rubyonracetracks.com/) and click on Installation -> Getting Started.
* You should be familiar with my [Rails From Scratch protocol](http://www.rubyonracetracks.com/rails_from_scratch.html) for starting apps.
* You should be comfortable using my [GenericApp gem](https://github.com/jhsu802701/generic_app) for starting new Ruby on Rails projects.

## What's the point?
This document shows how to give users the option of adding a profile picture.

## Why isn't this provided by GenericApp?
Not all apps require this feature.  Storing the profile pictures uses up disk space, which may be an issue if your app has a sufficiently large number of apps or if your production environment (such as a free Heroku account) gives you only a small amount of disk space.
