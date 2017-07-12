# Trulioo

This gem provides access to [Trulioo](https://www.trulioo.com/)'s GlobalGateway
API. For detailed information regarding the API, check out their [API
docs](https://api.globaldatacompany.com/docs).

## Getting Started

### Installation

Add `trulioo` to your Gemfile and run `bundle install`:

```ruby
gem 'trulioo`
```

Or install it manually:

```console
$ gem install trulioo
```

### Configure

Settings can be configured as so:

```ruby
require 'trulioo'

Trulioo.configure do |config|
  config.username = 'username'
  config.password = 'password'
end
```

### Usage

You'll want to try the `say_hello` to make sure the URL is correct and the
`test_authentication` endpoint to make sure your credientials are correct.

```ruby
client = Trulioo::Client.new
client.say_hello              # => "Hello World"
client.test_authentication    # => "Hello Company_Username"
```

## API Endpoints

The endpoints are accessed directly, meaning you do not call the API namespace
before the name of the endpoint. This may change in the future.

### Connection

Used to test your connection.

#### `say_hello`

This is the only endpoint that does not require authentication. It takes in an
optional param that will be displayed in the greeting.

```ruby
client.connection.say_hello            # => "Hello World"
client.connection.say_hello 'Company'  # => "Hello Company"
```

#### `test_authentication`

If the provided username and password are correct, this will return the username
of your account in the greeting.

```ruby
client.connection.test_authentication  # => "Hello Company_Username"
```

### Verifications

Accesses version 1 of their Normalized API.

#### `transaction_record`

Retrieves the information of an existing verification. This takes an optional
param if you would like more information. The options available:

- `:withaddress`: If your account includes address cleansing.
- `:verbose`: If your account includes address cleansing and watchlist details.

```ruby
client.verifications.transaction_record(transaction_id)
client.verifications.transaction_record(transaction_id, :withaddress)
client.verifications.transaction_record(transaction_id, :verbose)
```

#### `verify`

Performs a verification. Accepts a hash for the data.

```ruby
client.verifications.verify({ CountryCode: 'US', ... })
```

### Configuration

Information regarding how your account is configured.

#### `consents`

Consents required for the provided country. Accepts a string or symbol.

```ruby
client.configuration.consents('US')
```

#### `country_codes`

Returns countries configured for your account.

```ruby
client.configuration.consents
```

#### `country_subdivisions`

Gets the provinces states or other subdivisions for a country, mostly matches
ISO 3166-2.

```ruby
client.configuration.country_subdivisions('US')
```

#### `document_types`

Get available document verification types.

```ruby
client.configuration.document_types('US')
```

#### `fields`

Generates json schema for the API, which can be used to format your `data`
object when performing a verification.

```ruby
client.configuration.fields('US')
```
