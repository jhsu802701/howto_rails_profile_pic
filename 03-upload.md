# Chapter 3: Allowing Users to Upload Profile Pictures

In this chapter, you will give users the option of uploading a profile picture.

## New Branch
Enter the command "git checkout -b 03-upload".

## Integration Test
* Enter the command "rails generate integration_test users_picture_upload".
* Replace the contents of the test/integration/users_picture_upload.rb file with the following:
```
# rubocop:disable Metrics/AbcSize
# rubocop:disable Metrics/BlockLength
# rubocop:disable Metrics/MethodLength
require 'test_helper'

# NOTE: I could not figure out how to use Capybara to test for
# the process of uploading a profile picture.
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

  test 'user can sign up with profile picture' do
    get new_user_registration_path
    assert_select 'input[type=file]'
    p = fixture_file_upload('app/assets/images/higgins.jpg', 'image/jpg')
    post user_registration_path,
         params: { user: { username: 'csagan',
                           last_name: 'Sagan',
                           first_name: 'Carl',
                           email: 'carl_sagan@example.com',
                           picture: p,
                           password: 'googolplex',
                           password_confirmation: 'googalplex' } }
    u = User.find_by(username: 'csagan')
    assert u.picture?
    get user_confirmation_path(confirmation_token: u.confirmation_token)
    follow_redirect!
    msg = 'Your email address has been successfully confirmed.'
    assert_match msg, response.body
    get new_user_session_path
    post user_session_path,
         params: { user: { username: 'csagan',
                           password: 'googalplex' } }
    get user_path(u)
    bn = 'sagan.jpg'
    url = "/uploads/user/picture/#{u.id}/#{bn}"
    assert_select 'img' do
      assert_select '[src=?]', url
    end
  end

  test 'user can edit profile picture' do
    f1 = 'test/fixtures/files/connery1.jpeg'
    edit_picture(@u1, f1, 'Goldfinger')
    f2 = 'test/fixtures/files/connery2.jpg'
    edit_picture(@u1, f2, 'Goldfinger')
  end
end
# rubocop:enable Metrics/AbcSize
# rubocop:enable Metrics/BlockLength
# rubocop:enable Metrics/MethodLength
```
* Enter the command "sh build_fast.sh".  Both new integration tests will fail.
* Enter the command "alias test1='(command for repeating the tests that failed minus the TESTOPTS portion)'".
* Enter the command "test1".  Both tests fail because the user registration edit form and the user registration signup form lack the option of uploading a file.

## User Edit Form
* In the app/views/users/registrations/edit.html.erb file, add the following lines immediately before the username section:
```
  <div class="field">
    <span class="picture">
      <b>OPTIONAL: Upload profile picture</b>
      <%= f.file_field :picture, accept: 'image/jpeg,image/gif,image/png' %>
    </span>
  </div>
```
* In the app/views/users/registrations/edit.html.erb file, add the following lines immediately before the account cancellation section:
```
<script type="text/javascript">
  $('#micropost_picture').bind('change', function() {
    var size_in_megabytes = this.files[0].size/1024/1024;
    if (size_in_megabytes > 5) {
      alert('Maximum file size is 5MB. Please choose a smaller file.');
    }
  });
</script>
```

## User Signup Form
* In the app/views/users/registrations/new.html.erb file, add the following lines between the email section and the password section:
```
  <div class="field">
    <span class="picture">
      <b>OPTIONAL: Upload avatar</b>
      <%= f.file_field :picture, accept: 'image/jpeg,image/gif,image/png' %>
    </span>
  </div>
```
* In the app/views/users/registrations/new.html.erb file, add the following lines immediately before the "render" line:
```
<script type="text/javascript">
  $('#micropost_picture').bind('change', function() {
    var size_in_megabytes = this.files[0].size/1024/1024;
    if (size_in_megabytes > 5) {
      alert('Maximum file size is 5MB. Please choose a smaller file.');
    }
  });
</script>
```
* Enter the command "test1".  Both tests now fail because the expected picture files are unavailable.

## Profile Pictures For Test
* Enter the following commands:
```
mkdir test/fixtures/files/
touch test/fixtures/files/.keep
```
* Enter the following command to download a photo of Carl Sagan for the registration form test:
```
curl -o test/fixtures/files/rails.png -OL railstutorial.org/rails.png
```

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
