# Chapter 1: Creating The App

* Install the GenericApp gem by entering the command "gem install generic_app".
* Enter the following commands:
```
DATE=`date +%Y-%m-%d-%H%M-%S-%N`
APP_NAME="tmp-avatar-$DATE"
echo $APP_NAME
```
* Enter the command "generic_app".  When prompted for the name of your new app's directory, copy and paste the output of the above echo command.  When prompted for the name of your new app, enter "Temporary Avatar App".
* Follow the instructions provided for getting started and deploying to Heroku.  Because this is just a training exercise with a focus on user profile pictures, you can skip the continous integration badges and the updating of the static pages to save time.
* After you have completed these tasks and pushed them to your Git repository ahd Heroku, you are ready to move on.
