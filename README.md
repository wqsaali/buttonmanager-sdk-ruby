# Button Manager SDK

The PayPal Button Manager SDK provides Ruby APIs to create, and manage PayPal Payments Standard buttons programmatically.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'paypal-sdk-buttonmanager-rails'
```

And then execute:

```sh
$ bundle
```

Or install it yourself as:

```sh
$ gem install paypal-sdk-buttonmanager-rails
```

## Configuration

For Rails application:

```sh
rails g paypal:sdk:install
```

For other ruby application, create a configuration file(`config/paypal.yml`):

```yaml
development: &default
  username: jb-us-seller_api1.paypal.com
  password: WX4WTU3S8MY44S7F
  signature: AFcWxV21C7fd0v3bYYYRCpSSRl31A7yDhhsPUU2XhtMoZXsWHFxu-RWy
  app_id: APP-80W284485P519543T
  http_timeout: 30
  mode: sandbox
  sandbox_email_address: Platform.sdk.seller@gmail.com
  # # with certificate
  # cert_path: "config/cert_key.pem"
  # # with token authentication
  # token: ESTy2hio5WJQo1iixkH29I53RJxaS0Gvno1A6.YQXZgktxbY4I2Tdg
  # token_secret: ZKPhUYuwJwYsfWdzorozWO2U9pI
  # # with Proxy
  # http_proxy: http://proxy-ipaddress:3129/
  # # with device ip address
  # device_ipaddress: "127.0.0.1"
test:
  <<: *default
production:
  <<: *default
  mode: live
```

Load Configurations from specified file:

```ruby
PayPal::SDK::Core::Config.load('config/paypal.yml',  ENV['RACK_ENV'] || 'development')
```

## Example

```ruby
require 'paypal-sdk-buttonmanager-rails'
@api = PayPal::SDK::ButtonManagerRails::API.new(
  :mode      => "sandbox",  # Set "live" for production
  :app_id    => "APP-80W284485P519543T",
  :username  => "jb-us-seller_api1.paypal.com",
  :password  => "WX4WTU3S8MY44S7F",
  :signature => "AFcWxV21C7fd0v3bYYYRCpSSRl31A7yDhhsPUU2XhtMoZXsWHFxu-RWy" )

# Build request object
@bm_create_button = @api.build_bm_create_button({
  :ButtonType => "BUYNOW",
  :ButtonCode => "HOSTED",
  :ButtonVar => ["item_name=Testing","amount=5","return=http://localhost:3000/samples/button_manager/bm_create_button","notify_url=http://localhost:3000/samples/button_manager/ipn_notify"] })

# Make API call & get response
@bm_create_button_response = @api.bm_create_button(@bm_create_button)

# Access Response
if @bm_create_button_response.success?
  @bm_create_button_response.Website
  @bm_create_button_response.Email
  @bm_create_button_response.HostedButtonID
else
  @bm_create_button_response.Errors
end
```

