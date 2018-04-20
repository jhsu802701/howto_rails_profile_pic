# Ruby on Racetracks Tutorial: Adding User Profile Pictures

## Prerequisites
* Docker should be installed.  This is covered in the [Docker Tutorial](https://github.com/rubyonracetracks/tutorial-docker-stretch).
* You should have one of my rails-general Docker images installed.  This is covered in the [Docker Tutorial](https://github.com/rubyonracetracks/tutorial-docker-stretch).
* You should be familiar with the process of creating a new Rails app.  This is covered in the [GenericApp Tutorial](https://github.com/rubyonracetracks/tutorials-rails-short/blob/master/generic_app.md).

## What's the point?
This document shows how to give users the option of adding a profile picture.

## Why isn't this provided by GenericApp?
Not all apps require this feature.  Storing the profile pictures uses up disk space, which may be an issue if your app has a sufficiently large number of apps or if your production environment (such as a free Heroku account) gives you only a small amount of disk space.  Storing profile pictures in the cloud is an option, but the free deal from AWS expires after a year.
