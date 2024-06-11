# Step 1: Add 'dotenv-rails' and 'postmark-rails' gems to your Gemfile
```
gem 'dotenv-rails', '~> 3.1', '>= 3.1.2', groups: [:development, :test]
gem 'postmark-rails', '~> 0.22.1'

# Questions:
# 1: What is dotenv-rails? what does it help achieve?
# 2: What is postmark?
```

# Step 2: Run bundle install to install the gems
```
# $ bundle install
```

# Step 3: Edit your rails developement credentials
```
EDITOR="code --wait" rails credentials:edit


# Questions:
# 1: Why do we need credentails file
# 2: How would you use values saved in this file?
```

# Step 4: Add your Postmark API token to the credentials file
```

postmark_api_token: request-api-token-from-codehance
```

# Step 5: Create or update the configuration files for Action Mailer to use Postmark and load the API token from the .env file
```
# config/environments/development.rb
Rails.application.configure do
  # Other configurations...

  config.action_mailer.delivery_method = :postmark
  config.action_mailer.postmark_settings = { api_token: Rails.application.credentials.postmark_api_token }
  config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
end
```

# config/environments/production.rb
```
Rails.application.configure do
  # Other configurations...

  config.action_mailer.delivery_method = :postmark
  config.action_mailer.postmark_settings = { api_token: Rails.application.credentials.postmark_api_token }
  config.action_mailer.default_url_options = { host: 'yourdomain.com' }
end
```

# Step 6: Create a mailer using Rails generator
```
rails generate mailer UserMailer

# Questions:
# 1: What files are generated?
# 2: What is the purpose of generated files
```

# Step 7: Define the mailer actions in app/mailers/user_mailer.rb
```
class UserMailer < ApplicationMailer
  default from: 'name@domain.com'

  def otp_email(user, otp)
    @user = user
    @otp = otp

    mail(
      to: @user.email, 
      subject: 'Your OTP for Deal Sourcing',
      track_opens: 'true'
    )
  end
end
```

# Step 8: Create the email view in app/views/user_mailer/otp_email.html.erb
```
touch app/views/user_mailer/otp_email.html.erb

<div style="width: 100%; max-width: 600px; margin: 0 auto; background-color: #ffffff; padding: 20px; box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);">
  <div style="text-align: center; padding: 10px 0;">
    <h1 style="margin: 0; font-size: 24px; color: #333333;">Your OTP Code</h1>
  </div>
  <div style="padding: 20px 0;">
    <p style="font-size: 16px; color: #555555; line-height: 1.5;">Hello <%= @user.full_name %>,</p>
    <p style="font-size: 16px; color: #555555; line-height: 1.5;">You requested to change your password. Use the OTP code below to confirm your identity and proceed with the password change:</p>
    <div style="display: block; width: fit-content; margin: 20px auto; padding: 10px 20px; font-size: 24px; font-weight: bold; color: #ffffff; background-color: #4CAF50; border-radius: 5px;">
      <%= @otp %>
    </div>
    <p style="font-size: 16px; color: #555555; line-height: 1.5;">If you did not request this change, please ignore this email.</p>
    <p style="font-size: 16px; color: #555555; line-height: 1.5;">Thank you,<br>Deal Sourcing App Team</p>
  </div>
  <div style="text-align: center; padding: 20px 0; font-size: 14px; color: #aaaaaa;">
    <p>&copy; <%= Time.now.year %> Deal Sourcing All rights reserved.</p>
  </div>
</div>
```

# Step 9: Restart your Rails server to apply the changes
```
rails server
```
