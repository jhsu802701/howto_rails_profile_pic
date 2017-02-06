# Chapter 2: Showing User Profile Pictures
In this chapter, you will update the user model to allow profile pictures, seed the database, and update the user profile page to show the profile pictures.  While users will be able to have profile pictures, the process of allowing users to select a profile picture will be tackled later.  Although no tests are added in this chapter, testing will also be tackled later, and the ability to see the profile pictures at the end of this chapter will provide informal confirmation that everything works as expected.

## Git Branch
Enter the command "git checkout -b 02-show".

## Gemfile
* Add the following lines to the end of the Gemfile:
```
# BEGIN: for user profile pictures
gem 'carrierwave' # For uploading files
gem 'mini_magick' # For resizing images
# END: for user profile pictures
```
* Enter the command "sh git_check.sh".  All tests should pass, and there should be no offenses.
* Enter the following commands:
```
git add .
git commit -m "Added carrierwave and mini_magick gems for user profile pictures"
```
## .gitignore
* Add the line "public/uploads/" to the end of the .gitignore file.  This is necessary to keep the uploaded and seeded pictures out of the source code.
* Enter the following commands:
```
git add .
git commit -m "Keep uploaded and seeded pictures out of the source code"
```

## Adding the Picture Uploader
* Enter the command "rails generate uploader Picture".
* Enter the command "bundle exec rubocop -D".  You'll see offenses in app/uploaders/picture_uploader.rb.
* Edit the .rubocop.yml file, and add app/uploaders/picture_uploader.rb to the list of files excluded from AllCops.
* Enter the command "bundle exec rubocop -D".  There should be no offenses.
* Enter the command "sh git_check.sh".
* Enter the following commands:
```
git add .
git commit -m "Added the picture uploader"
```

## Adding the Picture Parameter to the User Model
* Enter the following commands:
```
rails generate migration add_picture_to_users picture:string
bin/rails db:migrate RAILS_ENV=test
rails db:migrate
```
* Enter the command "sh testm.sh".  All tests should pass.
* Enter the command "sh git_check.sh".  All tests should pass, and there should be no offenses.
* Enter the following commands:
```
git add .
git commit -m "Added the picture parameter to the user model"
```
## Updating the User Model
* Add the line "mount_uploader :picture, PictureUploader" to the app/models/user.rb file just before the private section.  The file should look like this:
```
. . . .

class User < ApplicationRecord
  . . . . 

  # Allows the file uploading process to fill in the picture parameter
  mount_uploader :picture, PictureUploader

  private

  . . . .
```
* Enter the command "sh git_check.sh".  All tests should pass, and there should be no offenses.
* Enter the following commands:
```
git add .
git commit -m "Allowed the file uploading process to fill in the picture parameter"
```

## Seeding the Database
* Edit the file db/seeds.rb.  Replace the entire user section with the following:
```
puts "Users: #{User.count}/101"

User.create!(last_name: 'Arroway', first_name: 'Ellie',
             username: 'earroway',
             email: 'ellie_arroway@rubyonracetracks.com',
             password: '3.14159265',
             password_confirmation: '3.14159265',
             confirmed_at: Time.now)

puts "Users: #{User.count}/101"

10.times do |n|
  name_l = Faker::Name.last_name
  name_f = Faker::Name.first_name
  email_address = "user#{n + 1}@rubyonracetracks.com"

  User.create!(last_name: name_l, first_name: name_f,
               username: "user-pic-#{n + 1}", email: email_address,
               password: 'Daytona 500',
               password_confirmation: 'Daytona 500',
               remote_picture_url: Faker::Avatar.image,
               confirmed_at: Time.now)
  print " #{User.count} "
end

90.times do |n|
  name_l = Faker::Name.last_name
  name_f = Faker::Name.first_name
  email_address = "user-faker-#{n + 1}@rubyonracetracks.com"

  User.create!(last_name: name_l, first_name: name_f,
               username: "user-nopic-#{n + 1}", email: email_address,
               password: 'Daytona 500',
               password_confirmation: 'Daytona 500',
               confirmed_at: Time.now)
  print " #{User.count} " if (User.count % 10).zero?
end

puts "Users: #{User.count}/101"
```
* Enter the command "sh seed.sh".
* Enter the command "sh git_check.sh".
* Enter the following commands:
```
git add .
git commit -m "Updated the seeding script to give profile pictures to a few users"
```

# Updating the User Profile Page
* Edit the app/views/users/show.html.erb file.  Add the profile picture display just before the delete button.  Your code should look like this:
```
    . . . .

    <br>
    <% if @user.picture? %>
      <%= image_tag @user.picture.url %>
    <% end %>
    <br>
    <br>
    <% if admin_signed_in? %>
      . . . .
    <% end %>
  </section>
</div>
```
* Stop the local server, and then restart it.  (If you neglect to do this, your browser will show an error message.)
* In your browser, log in as an admin, go to the user index, and view the user profile of any user with a username that begins with "user-pic".
* Enter the following commands:
```
git add .
git commit -m "Updated the user profile page to show profile pictures"
```

## Wrapping Up
* Enter the command "git push origin 02-show".
* Go to the GitHub repository and click on the "Compare and pull request" button for this branch.
* Accept this pull request to merge it with the master branch, but do NOT delete this branch.
* Enter the following commands:
```
git checkout master
git pull
sh heroku.sh
```
