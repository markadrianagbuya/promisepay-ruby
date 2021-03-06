# Ruby SDK - PromisePay API

[![Join the chat at https://gitter.im/PromisePay/promisepay-ruby](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/PromisePay/promisepay-ruby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

[![Gem Version](https://badge.fury.io/rb/promisepay.svg)](http://badge.fury.io/rb/promisepay)
[![Build Status](https://travis-ci.org/PromisePay/promisepay-ruby.svg?branch=develop)](https://travis-ci.org/PromisePay/promisepay-ruby)
[![Coverage Status](https://coveralls.io/repos/PromisePay/promisepay-ruby/badge.svg?branch=develop)](https://coveralls.io/r/PromisePay/promisepay-ruby?branch=develop)
[![Code Climate](https://codeclimate.com/github/PromisePay/promisepay-ruby/badges/gpa.svg)](https://codeclimate.com/github/PromisePay/promisepay-ruby)

#1. Installation

Add this line to your application's Gemfile:

```ruby
gem 'promisepay'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install promisepay

#2. Configuration

Before interacting with Promispay API you need to generate an access token.

See http://docs.promisepay.com/v2.2/docs/request_token for more information.

**Create a PromisePay client**

```ruby
require 'promisepay'

client = Promisepay::Client.new(username: 'YOUR_USERNAME', token: 'YOUR_TOKEN')
```

The client can be configured through environment variables.

```ruby
ENV['PROMISEPAY_USERNAME'] = 'YOUR_USERNAME'
ENV['PROMISEPAY_TOKEN'] = 'YOUR_TOKEN'
```

```ruby
require 'promisepay'

client = Promisepay::Client.new()
```

The following parameters are configurable through the client:

  * `:username` / `ENV['PROMISEPAY_USERNAME']`: username for [basic authentication](http://docs.promisepay.com/v2.2/docs/overview-2)
  * `:token` / `ENV['PROMISEPAY_TOKEN']`: token for [basic authentication](http://docs.promisepay.com/v2.2/docs/overview-2)
  * `:environment` / `ENV['PROMISEPAY_ENVIRONMENT']`: API [environment](http://docs.promisepay.com/v2.2/docs/environments) to use (default: 'test')
  * `:api_domain` / `ENV['PROMISEPAY_API_DOMAIN']`: API domain name to use (default: 'api.promisepay.com')

#3. Examples

##Tokens
##### Example 1 - Request session token
The below example shows the request for a marketplace configured to have the Item and User IDs generated automatically for them.

```ruby
token_request = client.tokens.create(:session, {
  current_user: 'seller',
  item_name: 'Test Item',
  amount: '2500',
  seller_lastname: 'Seller',
  seller_firstname: 'Sally',
  buyer_lastname: 'Buyer',
  buyer_firstname: 'Bobby',
  buyer_country: 'AUS',
  seller_country: 'USA',
  seller_email: 'sally.seller@promisepay.com',
  buyer_email: 'bobby.buyer@promisepay.com',
  fee_ids: [],
  payment_type_id: 2
})
token = token_request['token']
item_id = token_request['item']
buyer_id = token_request['buyer']
seller_id = token_request['seller']
```
#####Example 2 - Request session token
The below example shows the request for a marketplace that passes the Item and User IDs.

```ruby
token = client.tokens.create(:session, {
  current_user_id: 'seller1234',
  item_name: 'Test Item',
  amount: '2500',
  seller_lastname: 'Seller',
  seller_firstname: 'Sally',
  buyer_lastname: 'Buyer',
  buyer_firstname: 'Bobby',
  buyer_country: 'AUS',
  seller_country: 'USA',
  seller_email: 'sally.seller@promisepay.com',
  buyer_email: 'bobby.buyer@promisepay.com',
  external_item_id: 'TestItemId1234',
  external_seller_id: 'seller1234',
  external_buyer_id: 'buyer1234',
  fee_ids: [],
  payment_type_id: 2
})['token']
```
##Items

#####Create an item
```ruby
item = client.items.create(
  id: '12345',
  name: 'test item for 5AUD',
  amount: '500',
  payement_type: '1',
  buyer_id: buyer.id,
  seller_id: seller.id,
  fee_id: fee.id,
  description: '5AUD transfer'
)
```
#####Get an item
```ruby
item = client.items.find('1')
```
#####Get a list of items
```ruby
items = client.items.find_all
```
#####Update an item
```ruby
item.update(name: 'new name')
```
#####Cancel an item
```ruby
item.cancel
```
#####Get an item status
```ruby
item.status
```
#####Get an item's buyer
```ruby
item.buyer
```
#####Get an item's seller
```ruby
item.seller
```
#####Get an item's fees
```ruby
item.fees
```
#####Get an item's transactions
```ruby
item.transaction
```
#####Get an item's wire details
```ruby
item.wire_details
```
#####Get an item's BPAY details
```ruby
item.bpay_details
```

##Users

#####Create a user
```ruby
user = client.users.create(
  id: '123456',
  first_name: 'test',
  last_name: 'buyer',
  email: 'buyer@test.com',
  address_line1: '48 collingwood',
  state: 'vic',
  city: 'Mel',
  zip: '3000',
  country: 'AUS'
)
```
#####Get a user
```ruby
user = client.users.find('1')
```
#####Get a list of users
```ruby
users = client.users.find_all
```
#####Get a user's card accounts
```ruby
user.card_account
```
#####Get a user's PayPal accounts
```ruby
user.paypal_account
```
#####Get a user's bank accounts
```ruby
user.bank_account
```
#####Get a user's items
```ruby
user.items
```
#####Set a user's disbursement account
```ruby
user.disbursement_account(bank_account.id)
```
##Item Actions
#####Make payment
```ruby
item.make_payment(
  account_id: buyer_card_account.id
)
```
#####Request payment
```ruby
item.request_payement
```
#####Release payment
```ruby
item.release_payment
```
#####Request release
```ruby
item.request_release
```
#####Cancel
```ruby
item.cancel
```
#####Acknowledge wire
```ruby
item.acknowledge_wire
```
#####Acknowledge PayPal
```ruby
item.acknowledge_paypal
```
#####Revert wire
```ruby
item.revert_wire
```
#####Request refund
```ruby
item.release_payment
```
#####Refund
```ruby
item.request_refund(
  refund_amount: '1000',
  refund_message: 'because'
)
```
##Card Accounts
#####Create a card account
```ruby
card_account = client.card_accounts.create(
  user_id: buyer.id,
  full_name: 'test Buyer',
  number: '4111 1111 1111 1111',
  expiry_month: Time.now.month,
  expiry_year: Time.now.year + 1,
  cvv: '123'
)
```
#####Get a card account
```ruby
card_account = client.card_accounts.find('1')
```
#####Deactivate a card account
```ruby
card_account.deactivate
```
#####Get a card account's users
```ruby
card_account.user
```

##Bank Accounts
#####Create a bank account
```ruby
bank_account = client.bank_accounts.create(
  user_id: seller.id,
  bank_name: 'Nab',
  account_name: 'test seller',
  routing_number: '22222222',
  account_number: '1234567890',
  account_type: 'savings',
  holder_type: 'personal',
  country: 'AUS'
)
```
#####Get a bank account
```ruby
bank_account = client.bank_accounts.find('1')
```
#####Deactivate a bank account
```ruby
bank_account.deactivate('mobile_pin')
```
#####Get a bank account's users
```ruby
bank_account.user
```

##PayPal Accounts
#####Create a PayPal account
```ruby
paypal_account = client.paypal_accounts.create(
  user_id: seller.id,
  paypal_email: 'seller@promisepay.com'
)
```
#####Get a PayPal account
```ruby
paypal_account = client.paypal_accounts.find('1')
```
#####Deactivate a PayPal account
```ruby
paypal_account.deactivate
```
#####Get a PayPal account's users
```ruby
paypal_account.user
```

##Fees
#####Get a list of fees
```ruby
fees = client.fees.find_all
```
#####Get a fee
```ruby
fees = client.fees.find('1')
```
#####Create a fee
```ruby
fee = client.fees.create(
  name: 'test fee for 5 AUD',
  fee_type_id: '1',
  amount: '75',
  to: 'seller'
)
```

##Transactions
#####Get a list of transactions
```ruby
transactions = client.transactions.find_all
```
#####Get a transaction
```ruby
transaction = client.transactions.find('1')
```
#####Get a transaction's users
```ruby
transaction.users
```
#####Get a transaction's fees
```ruby
transaction.fees
```

_Check out the [online documentation](http://promisepay.github.io/promisepay-ruby/) to get a full list of available resources and methods._

#4. Contributing

	1. Fork it ( https://github.com/PromisePay/promisepay-ruby/fork )
	2. Create your feature branch (`git checkout -b my-new-feature`)
	3. Commit your changes (`git commit -am 'Add some feature'`)
	4. Push to the branch (`git push origin my-new-feature`)
	5. Create a new Pull Request
