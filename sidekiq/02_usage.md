!SLIDE smbullets
# Using Sidekiq from Resque

- Uses the same message format in Redis
- Resque client API can push messages onto Redis for Sidekiq to process
- Supports resque-web if redis namespace is set for resque

!SLIDE
# Job Class
In Resque

    @@@ ruby
    class MyWorker
      def self.perform(name, count)
        # do something
      end
    end

In Sidekiq
    
    @@@ ruby
    class MyWorker
      inlcude Sidekiq::Worker
      def perform(name, count)
        # do something
      end
    end

!SLIDE
# Client API
Resque:
    @@@ ruby
    Resque.enqueue(MyWorker, 'bob', 1)

Sidekiq:
    @@@ ruby
    MyWorker.perform_async('bob', 1)

!SLIDE
# Runinning workers

    @@@ sh
    $ sidekiq

    # define number of concurrent threads
    $ sidekiq -c 10

!SLIDE smbullets
# Now has capistrano integration

config/deploy.rb

    @@@ ruby small
    require 'sidekiq/capistrano'

    set :sidekiq_role, :sidekiq
    role :sidekiq, 'worker-1.acmecorp.com', 'worker-2.acmecorp.com'

!SLIDE smbullets
# Gotchas

- Make sure you have the right size ActiveRecord pool size
- Beware of what you're background task is, CPU work on Sidekiq
  might not play nice
