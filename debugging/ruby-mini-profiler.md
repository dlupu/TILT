
#Using `rack-mini-profiler` to track memory leaks

My application has a memory leap. I've tried identifing the source of the leak by deactivating features of the application but did not managed to find where the problem was coming from.

And then I discovered the `ruby-mini-profiler` gem that gives a lot of insights on the memory.

##Installing the gem

```ruby 
# add this to Gemfile 
# in my case the dev environment is an environment identical to the production one (but used to test branches under development)
group :dev, :development do
  # source https://www.nateberkopec.com/2015/08/05/rack-mini-profiler-the-secret-weapon.html
  gem 'rack-mini-profiler'
  gem 'flamegraph'
  gem 'stackprof' # ruby 2.1+ only
  gem 'memory_profiler'
end
```

According to one of the articles in the sources it's best to disable https. In my case that meant I needed to change `FORCE_SSL` value for `'1'`to `'0'`and also tell the browser to [forget about the STS settings](../chrome/clear_hsts_state.md).

We also need to authorize the gem in order to be used. For simplicity I decided to make it available to all user :

```ruby 
# app/controllers/application_controller.rb
before_action do
  Rack::MiniProfiler.authorize_request if Rails.env.dev? || Rails.env.development?
end
```

The gem requires storage. For simplicity reasons I decided to use the memory (but you can also use redis or filestorage). 

```ruby
# config/initializers/rack_mini_profiler.rb
if Rails.env.dev? || Rails.env.development?
 (Rack::MiniProfiler.config.storage = Rack::MiniProfiler::MemoryStore) if Rails.env.dev? || Rails.env.development?
end
```

Now you can access the miniprofiler page by adding `?pp=help` to the end of the url. Most useful features for debugging memory issues are:
* profile-gc 
* profile-memory 
* analyze-memory 

Sources
* [rack-mini-profiler gem](https://github.com/miniprofiler/rack-mini-profiler)
* [rack-mini-profiler - the Secret Weapon of Ruby and Rails Speed](https://www.nateberkopec.com/2015/08/05/rack-mini-profiler-the-secret-weapon.html)
* [Flame graphs in ruby-mini-profiler](https://samsaffron.com/archive/2013/03/19/flame-graphs-in-ruby-miniprofiler)
