# productionx
EDITOR="code --wait" rails credentials:edit --environment production
config/credentials/production.key
config/credentials/production.yml.enc

EDITOR="code --wait" rails credentials:edit --environment development
config/credentials/development.key
config/credentials/development.yml.enc

# without RAILS_ENV: will result to production ready
EDITOR="code --wait" rails credentials:edit

Rails.application.credentials.secret_key_base

RAILS_ENV=development EDITOR="code --wait" rails credentials:edit 
config/credentials

ENV['RAILS_MASTER_KEY']