$ docker-compose run web rails new . --force --database=postgresql

$ docker-compose build

Replace the contents of config/database.yml with the following:

default: &default
  adapter: postgresql
  encoding: unicode
  host: db
  username: <%= ENV["POSTGRES_USER"] %>
  password: <%= ENV["POSTGRES_PASSWORD"] %>
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>

development:
  <<: *default
  database: app_development

test:
  <<: *default
  database: app_test

production:
  <<: *default 
  host: <%= ENV["POSTGRES_HOST"] %>
  database: app_production


$ docker-compose run web rake db:create


To restart the application:

Run 

$ docker-compose up 

in the project directory.
Run this command in another terminal to restart the database: 

$ docker-compose run web rake db:create

If you make changes to the Gemfile or the Compose file to try out some different configurations, you will need to rebuild. Some changes will require only 
$ docker-compose up --build
but a full rebuild requires a re-run of 

$ docker-compose run web bundle install 

to sync changes in the Gemfile.lock to the host, followed by 

$ docker-compose up --build