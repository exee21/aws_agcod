# AwsAgcod

Amazon Gift Code On Demand (AGCOD) API v2 implementation for distribute Amazon gift cards (gift codes) instantly in any denomination.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'aws_agcod'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install aws_agcod

## Usage

#### Configure

```ruby
require "aws_agcod"

AwsAgcod.configure do |config|
  config.access_key = "YOUR_ACCESS_KEY"
  config.secret_key = "YOUR_SECRET_KEY"
  config.partner_id = "PARTNER_ID"
  config.uri        = "https://agcod-v2-gamma.amazon.com" # default, sandbox endpoint
  config.partner_id = "us-east-1" # default
end
```

#### Create Gift Code/Card

```ruby
request_id = "test"
amount = 10 # USD

request = AwsAgcod::CreateGiftCard.new(request_id, amount)

# When succeed
if request.success?
  request.claim_code # => code for the gift card
  request.gc_id # => gift card id
  request.request_id # => your request id
else
# When failed
  request.error_message # => Error response from AGCOD service
end
```

#### Cancel Gift Code/Card

```ruby
request_id = "test"
gc_id = "test_gc_id"

request = AwsAgcod::CancelGiftCard.new(request_id, gc_id)

# When failed
unless request.success?
  request.error_message # => Error response from AGCOD service
end
```

#### Get Gift Code/Card activities

```ruby
request_id = "test"
start_time = Time.now - 86400
end_time = Time.now
page = 1
per_page = 100
show_no_ops = false # Wehther to show activities with no operation or not

request = AwsAgcod::GiftCardActivityList.new(request_id, start_time, end_time, page, per_page, show_no_ops)

# When succeed
if request.success?
  request.results.each do |activity|
    activity.status # => SUCCESS, FAILURE, RESEND
    activity.created_at
    activity.type
    activity.card_number
    activity.amount
    activity.error_code
    activity.gc_id
    activity.partner_id
    activity.request_id
  end
else
# When failed
  request.error_message # => Error response from AGCOD service
end
```
## Contributing

1. Fork it ( https://github.com/[my-github-username]/aws_agcod/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request
