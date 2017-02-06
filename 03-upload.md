# Chapter 3: Allowing Users to Upload Profile Pictures

In this chapter, you will give users the option of uploading a profile picture.

## New Branch
Enter the command "git checkout -b 03-upload".

## Integration Test
* Enter the command "rails generate integration_test users_picture_upload".
* Replace the contents of the test/integration/users_picture_upload.rb file with the following:
```
require 'test_helper'

# NOTE: I could not figure out how to use Capybara to test for
# the process of uploading a picture to use for an avatar.
# The following tests are based on the Rails Tutorial procedure
# for adding a picture to a micropost.
class UsersPictureUploadTest < ActionDispatch::IntegrationTest
  # uo: user object
  # fn: filename of image file
  # pd: password
  def edit_picture(uo, fn, pd)
    basename = File.basename fn
    login_as(uo, scope: :user)
    get edit_user_registration_path(uo)
    assert_select 'input[type=file]'
    u = uo
    pic = fixture_file_upload(fn, 'image/jpg')
    patch user_registration_path,
          params: { user: { picture: pic,
                            current_password: pd } }
    msg = 'Your account has been updated successfully.'
    get user_path(u)
    assert_match msg, response.body
    url = "/uploads/user/picture/#{u.id}/#{basename}"
    assert_select 'img' do
      assert_select '[src=?]', url
    end
  end

  test 'user can signup with avatar' do
    get new_user_registration_path
    assert_select 'input[type=file]'
    p = fixture_file_upload('app/assets/images/higgins.jpg', 'image/jpg')
    post user_registration_path,
         params: { user: { username: 'jhiggins',
                           last_name: 'Higgins',
                           first_name: 'Jonathan',
                           email: 'jhiggins@example.com',
                           picture: p,
                           password: 'Zeus and Apollo',
                           password_confirmation: 'Zeus and Apollo' } }
    u = User.find_by(username: 'jhiggins')
    assert u.picture?
    get user_confirmation_path(confirmation_token: u.confirmation_token)
    follow_redirect!
    msg = 'Your email address has been successfully confirmed.'
    assert_match msg, response.body
    get new_user_session_path
    post user_session_path,
         params: { user: { username: 'jhiggins',
                           password: 'Zeus and Apollo' } }
    get user_path(u)
    bn = 'higgins.jpg'
    url = "/uploads/user/picture/#{u.id}/#{bn}"
    assert_select 'img' do
      assert_select '[src=?]', url
    end
  end

  test 'user can edit avatar' do
    f1 = 'test/fixtures/files/connery1.jpeg'
    edit_picture(@u1, f1, 'Goldfinger')
    f2 = 'test/fixtures/files/connery2.jpg'
    edit_picture(@u1, f2, 'Goldfinger')
  end
end
```

## User Edit Form

## User Signup Form

## User Registration Controller

## Wrapping Up
* Enter the command "git push origin 03-upload".
* Go to the GitHub repository and click on the "Compare and pull request" button for this branch.
* Accept this pull request to merge it with the master branch, but do NOT delete this branch.
* Enter the following commands:
```
git checkout master
git pull
sh heroku.sh
```
