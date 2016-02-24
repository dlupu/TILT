
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

```ruby
# config/initializers/rack_mini_profiler.rb
if Rails.env.dev? || Rails.env.development?
  # I'm using Redis as a storage
  Rack::MiniProfiler.config.storage_options = { url: "#{ENV['REDIS_URL']}/3" }
  Rack::MiniProfiler.config.storage = Rack::MiniProfiler::RedisStore
  
  # You can use memory storage if you don't have redis on your project. see gem readme for more information
end
```

Sources
* [rack-mini-profiler gem](https://github.com/miniprofiler/rack-mini-profiler)
* [rack-mini-profiler - the Secret Weapon of Ruby and Rails Speed](https://www.nateberkopec.com/2015/08/05/rack-mini-profiler-the-secret-weapon.html)
* [Flame graphs in ruby-mini-profiler](https://samsaffron.com/archive/2013/03/19/flame-graphs-in-ruby-miniprofiler)
