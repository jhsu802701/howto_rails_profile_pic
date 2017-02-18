# Chapter 3: Allowing Users to Upload Profile Pictures

In this chapter, you will give users the option of uploading a profile picture.

## New Branch
Enter the command "git checkout -b 03-upload".

## Integration Test
* Enter the command "rails generate integration_test users_picture_upload".
* Replace the contents of the test/integration/users_picture_upload.rb file with the following:
```
# rubocop:disable Metrics/AbcSize
# rubocop:disable Metrics/MethodLength
require 'test_helper'

class UsersPictureUploadTest < ActionDispatch::IntegrationTest
  include CarrierWaveDirect::Test::CapybaraHelpers

  # Xpath string used for testing for images
  def xpath_input_img(url)
    str1 = './/img[@src="'
    str2 = url
    str3 = '"]'
    output = "#{str1}#{str2}#{str3}"
    output
  end

  # uo: user object
  # fn: filename of image file
  # pd: password
  def edit_picture(uo, fn, pd)
    basename = File.basename fn
    login_as(uo, scope: :user)
    visit root_path
    click_on 'Edit Settings'
    find('form input[type="file"]').set(Rails.root + fn)
    fill_in('Current password', with: pd)
    click_button('Update')
    puts page.body
    # assert page.has_text?('Your account has been updated successfully.')
    click_on 'Your Profile'
    url = "/uploads/user/picture/#{uo.id}/#{basename}"
    page.assert_selector(:xpath, xpath_input_img(url))
  end

  test 'user can sign up with profile picture' do
    visit root_path
    click_on 'Sign up now!'
    fill_in('Last name', with: 'Sagan')
    fill_in('First name', with: 'Carl')
    fill_in('Username', with: 'csagan')
    fill_in('Email', with: 'carl_sagan@example.com')
    fill_in('Password', with: 'googolplex')
    fill_in('Password confirmation', with: 'googolplex')
    basename = 'sagan.jpg'
    find('form input[type="file"]').set(Rails.root + "test/fixtures/files/#{basename}")
    click_button('Sign up')
    open_email('carl_sagan@example.com')
    current_email.click_link 'Confirm my account'
    login_user('csagan', 'googolplex', false)
    click_on 'Your Profile'
    u = User.find_by(username: 'csagan')
    url = "/uploads/user/picture/#{u.id}/#{basename}"
    page.assert_selector(:xpath, xpath_input_img(url))
  end

  test 'user can edit profile picture' do
    f1 = 'test/fixtures/files/connery1.jpg'
    edit_picture(@u1, f1, 'Goldfinger')
    f2 = 'test/fixtures/files/connery2.jpg'
    edit_picture(@u1, f2, 'Goldfinger')
  end
end
# rubocop:enable Metrics/AbcSize
# rubocop:enable Metrics/MethodLength
```
* Enter the command "sh build_fast.sh".  Both new integration tests will fail.
* Enter the command "alias test1='(command for repeating the tests that failed minus the TESTOPTS portion)'".
* Enter the command "test1".  Both tests fail because the user registration edit form and the user registration signup form lack the option of uploading a file.

## User Forms
* In the app/views/users/registrations/edit.html.erb file, add the following lines immediately before the username section:
```
  <div class="field">
    <span class="picture">
      <b>OPTIONAL: Upload profile picture</b>
      <%= f.file_field :picture, accept: 'image/jpeg,image/gif,image/png' %>
    </span>
  </div>
```
* In the app/views/users/registrations/new.html.erb file, add the following lines between the email section and the password section:
```
  <div class="field">
    <span class="picture">
      <b>OPTIONAL: Upload profile picture</b>
      <%= f.file_field :picture, accept: 'image/jpeg,image/gif,image/png' %>
    </span>
  </div>
```
* Enter the command "test1".  Both tests now fail because the expected picture files are unavailable.

## Profile Pictures For Test
* Enter the following command to download a photo of Carl Sagan for the registration form test:
```
curl -o test/fixtures/files/sagan.jpg -OL https://upload.wikimedia.org/wikipedia/commons/e/e8/Sagan_Viking.jpg
```
* Enter the following commands to download photos of Sean Connery for the user registration edit form test:
```
curl -o test/fixtures/files/connery1.jpg -OL https://upload.wikimedia.org/wikipedia/commons/c/c8/Sean_Connery_1971_%28cropped%29.jpg
curl -o test/fixtures/files/connery2.jpg -OL https://upload.wikimedia.org/wikipedia/commons/d/d1/Sean_Connery_en_Micheline_Roquebrune_%281983%29.jpg
```
* Both tests fail because the profile pictures do not exist.  While the forms are present, the expected post-submission actions are not.

## User Registration Controller
* Edit the file app/controllers/users/registrations_controller.rb.  In the configure_sign_up_params and configure_account_update_params definitions, add ":picture" to the list of keys.

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
* Congratulations!  Your users now can have profile pictures.
