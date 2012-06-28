!SLIDE smbullets
# Deployment

- Deploying restarts workers
- Therefore manaing PIDs, no of processes
- Something that works well with Capistrano

!SLIDE smbullets
# Foreman
## https://github.com/ddollar/foreman

- Manage Procfile-based applications
- Heroku plays nice with Procfiles

!SLIDE
## assuming deploying to Linux that uses upstart

!SLIDE smbullets
# Procfile

A procfile defines processes for your application

    @@@ text
    worker: RAILS_ENV=production bundle exec rake resque:work QUEUE=*

then to run processes defined in Procfile
    @@@ sh
    $ foreman start

    # run up one type of process only
    $ foreman run worker

    # increase the number of processes
    $ foreman run worker -c 10

!SLIDE smbullets
# Exporting config

Foreman supports exporting to bluepill, inittab, runit, upstart

    @@@ text
    $ sudo foreman export upstart /etc/init

    # then allows
    $ sudo service start my_app
    $ sudo service restart my_app

!SLIDE smbullets
# Capistrano / Foreman

    @@@ ruby small
    namespace :foreman do
     desc "Export upstart scripts for Procfile processes"
     task :export, :roles => :app do
      command = "cd #{current_path}"
      command += "&& bundle exec foreman export upstart /tmp"
      command += "-a #{application} -u #{user} -l #{shared_path}/log"
      sudo "[ -f /etc/init/#{application}.conf ] || exit 0"
      sudo "rm /etc/init/#{application}*.conf"
      sudo "mv /tmp/#{application}*.conf /etc/init/"
     end

     desc "Restart the application services"
     task :restart, :roles => :app do
      sudo "start #{application} || restart #{application}"
     end
    end
    
    after "deploy:update", "foreman:export"
    after "deploy:update", "foreman:restart"
