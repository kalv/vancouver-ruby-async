!SLIDE
# Getting started

!SLIDE bullets
# Define a Job

Class with a class method `perform` to hold what you want to run

    @@@ Ruby
    class PostIndexer
      # Name of the queue this will be placed on
    	@queue = :activity_stream
    
      # what will be executed later by a worker
    	def self.perform(post_id)
    		post = Post.find(post_id)
    		post.index
    	end
    end

!SLIDE bullets
# Queue a job to be processed later

    @@@ Ruby
    class Post
      # Update search index after creation
    	after_create :async_index
    
      # Enqueue a PostIndexer job with this post id
    	def async_index
    		Resque.enqueue(PostIndexer, self.id)
    	end
    end

!SLIDE bullets
# Start a worker

Worker will pick up jobs and start to process them

    @@@ sh
    # Run work in a process that stays in foreground
    $ QUEUE=activity_stream rake resque work
    
    # Run up a number of workers
    $ QUEUE=activity_stream COUNT=10 rake resque:workers

!SLIDE smbullets
# Some worker options

    @@@ sh
    # poll frequency to Redis
    INTERVAL=0.1

    # What queues to process on the worker being started
    QUEUE=indexer,activity_stream,critical
    QUEUE=* to process all queues

    # Verbose logging
    VVERBOSE=1
