!SLIDE
# Testing

!SLIDE
# Testing the Job

Just test the class!

    @@@ Ruby
    describe PostIndexer do
      before do
        @post = Factory(:post)
        PostIndexer.new.perform(@post.id)
      end
      
      it 'should index a post' do
        Searcher.find(@post.title).first.should ==
          @post.title
      end
    end

!SLIDE
# Testing the enqueuing of a job

You can get either `resque_unit` for Test::Unit or `resque_spec` for
RSpec.

    @@@ Ruby
    before do
      ResqueSpec.reset!
    end
    
    it "create post adds a PostIndexer job to the queue" do
      Post.create!(Factory.attributes_for(:post))
      PostIndexer.should have_queue_size_of(1)
      PostIndexer.should have_queued(post.id)
    end

!SLIDE
# Integration testing

Given this cucumber scenario

    @@@ cucumber
    Given a user signed in
    When I enter a new post called "Ruby rocks"
    Then searching for "Ruby rocks" finds a result

The step would look like this

    @@@ Ruby
    When /I enter a new post called "([^"]*)"/ do |title|
      with_resque do
        create_a_new_post
        click_button 'create'
      end
    end

!SLIDE
# any questions?

### next up deployment
