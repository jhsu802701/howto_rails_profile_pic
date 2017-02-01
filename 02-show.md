# Chapter 2: Showing User Profile Pictures
In this chapter, you will update the user model to allow profile pictures, seed the database, and update the user profile page to show the profile pictures.  While users will be able to have profile pictures, the process of allowing users to select a profile picture will be tackled later.  Although no tests are added in this chapter, testing will also be tackled later, and the ability to see the profile pictures at the end of this chapter will provide informal confirmation that everything works as expected.

## Git Branch
Enter the command "git checkout -b 02-show".

## Adding the Picture Parameter to the User Model
* Enter the following commands:
```
rails generate migration add_picture_to_microposts picture:string
rails db:migrate
```
* Enter the command "sh testm.sh".  All tests should pass.
* Enter the command "sh git_check.sh".  All tests should pass, and there should be no offenses.
* Enter the following commands:
```
git add .
git commit -m "Added the picture parameter to the user model"
```

## Seeding the Database
* Edit the file db/seeds.rb.  In the second and third User.create! sections (the parts that add many users instead of just one), add the line "remote_picture_url: Faker::Avatar.image,".  The code should look something like this:
```
User.create!( . . . .
            . . . .
            . . . .)

puts . . . .

50.times do |n|
  . . . .

  User.create!( . . . .
               . . . .
               remote_picture_url: Faker::Avatar.image,
               confirmed_at: Time.now)
  print  . . . .
end

50.times do |n|
  . . . .
  User.create!( . . . .
               . . . .
               remote_picture_url: Faker::Avatar.image,
               confirmed_at: Time.now)
  print . . . .
end
```

# Updating the User Profile Page
* Edit the app/views/users/show.html.erb file.  Add the profile picture display just before the "</section>" tag.  Your code should look like this:
```
    . . . .
    <br>
    <% if @user.picture? %>
      <%= image_tag @user.picture.url %>
    <% end %>
    </section>
  </aside>
</div>
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
