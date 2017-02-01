# Chapter 1: Getting Started

## Creating the App
* Install the GenericApp gem by entering the command "gem install generic_app".
* Enter the following commands:
```
DATE=`date +%Y-%m-%d-%H%M`
APP_NAME="generic-avatar-$DATE"
echo $APP_NAME
```
* Enter the command "generic_app".  When prompted for the name of your new app's directory, copy and paste the output of the above echo command.  When prompted for the name of your new app, enter "Generic Avatar App".
* Follow the instructions provided for getting started and deploying to Heroku.

```
## OPTIONAL: Continuous Integration Badges
* Add continuous integration badges
  * [CircleCI](https://circleci.com/)
  * [Gemnasium](https://www.gemnasium.com/)
  * [Hakiri](https://hakiri.io/)
  * [CodeClimate GPA Rating](https://codeclimate.com/)
* For more details, go to [Rails From Scratch](http://www.rubyonracetracks.com/rails_from_scratch.html).
* Enter the following commands:
```
git add .
git commit -m "Added continuous integration badges"
git push origin master
```
