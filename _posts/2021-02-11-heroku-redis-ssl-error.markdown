---
layout: post
title:  "How to solve SSL error for Redis 6 on Heroku?"
date:   2021-02-11 12:00:00 +0300
permalink: /en/heroku-redis-ssl-error
categories: redis heroku ssl
---

> This article is also available on [Roam Research](https://roamresearch.com/#/app/ogirginc/page/6J78BW-ov).

If you try to connect a Ruby on Rails app with a Heroku Redis add-on (excluding the Hobby Dev plan), there is a very high chance for you to get the error below:

> OpenSSL::SSL::SSLError: SSL_connect returned=1 errno=0 state=error: 
certificate verify failed (self signed certificate in certificate chain)

<h2>Reason</h2>

From version 6 and above, Redis requires using TLS to connect. However, Heroku does not use SSL internally. They terminate SSL at the router level and [forward](https://devcenter.heroku.com/articles/http-routing#routing) requests from there to your application via HTTP which is safe as all these do happen behind Heroku's firewall. Also, let's face it, Heroku's security measures ~~probably~~ are better than yours.

<h2>Solution</h2>

To fix this, you will need to use `OpenSSL::SSL::VERIFY_NONE` for your Redis client.

```ruby
Redis.new(
  url: 'url',
  driver: :ruby,
  ssl_params: { verify_mode: OpenSSL::SSL::VERIFY_NONE }
)
```

If you do use Sidekiq, configuration should be done through the Sidekiq initializers:

```ruby
# config/initializers/sidekiq.rb
Sidekiq.configure_server do |config|
  config.redis = { ssl_params: { verify_mode: OpenSSL::SSL::VERIFY_NONE } }
end

Sidekiq.configure_client do |config|
  config.redis = { ssl_params: { verify_mode: OpenSSL::SSL::VERIFY_NONE } }
end
```

<h2>Troubleshooting</h2>

<h3>No error with Redis 6!?</h3>

<h4>Why?<h4>

Assuming you have double checked the Redis version, Redis plan is probably on the Hobby Dev version, which does support both HTTP and HTTPS [connections](https://devcenter.heroku.com/articles/heroku-redis#create-a-new-instance).

<h4>Solution<h4>

For the Hobby Dev plan, you should see two environment variables set under the app's Config Vars section. If you don't plan to upgrade the add-on to Premium 0 or above, no change is needed.

For the Hobby Dev plan, you should see two environment variables set under the app's Config Vars section. If you don't plan to upgrade the add-on to Premium 0 or above, no change is needed. 
