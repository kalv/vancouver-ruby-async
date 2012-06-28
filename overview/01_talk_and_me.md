!SLIDE 
# Asynchronous processing for Rails #

## with Resque ##

### Kalvir Sandhu June 2012 ###

!SLIDE smbullets
# What you're going to get #

* The problem
* Resque overview
* Getting started
* Testing Resque
* Deployment of Resque
* Sidekiq

!SLIDE bullets

# Me - Kalv
* Twitter - @kalv
* Blog - http://kalv.co.uk
* Working on Goodbits.co

!SLIDE smbullets
# Ruby async projects to date

* Built Twitterfeed's XMPP architecture - publishing > 5m RSS feeds to twitter, all in Ruby!
* Hashblue.com - UK O2 customers to access their SMS messages
  online and via an API.
* Recently Goodbits.co for handling RSS push notifications

!SLIDE smbullets
# Needs
- Need to offload long running tasks into the background
- Need to speed up http responses to improve user experiences

!SLIDE
## Sending email using a provider like GMail, Sendgrid in case it's slow

!SLIDE
## Posting data to Twitter, in case it's down or slow

!SLIDE
## Updating search indexes in your application

!SLIDE
## Too many after_create hooks that are querying your database a lot

!SLIDE
# Resque
## https://github.com/defunkt/resque
![resque-github](resque-github.png)

!SLIDE smbullets
# Resque Overview

* Built by Chris Wanstrath for Github's background processing
* Very stable and well written
* Uses Redis to pass jobs around architecture
* Dead simple job class definition

!SLIDE smbullets
# Resque components

- A library to help defining jobs to process
- Rake task to start workers to process jobs
- Also a sinatra application to monitor jobs enqueued and workers
- Workers can be distributed as it uses Redis to store jobs to be processed

