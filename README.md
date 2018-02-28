# africastalking-python

> The wrapper provides convenient access to the Africa's Talking API from applications written in Python.


## Documentation
Take a look at the [API docs here](http://docs.africastalking.com).

## Install

```bash
$ pip  install africastalking # python 2.x

OR

$ python -m pip install africastalking # python 3.x

OR

$ pip3 install africastalking # python 3.x

OR

$ python3 -m pip install africastalking # python 3.x

```


## Usage

```python
# import package
import africastalking


# Initialize SDK
username = "YOUR_USERNAME"    # use 'sandbox' for development in the test environment
api_key = "YOUR_API_KEY"      # use your sandbox app API key for development in the test environment
africastalking.initialize(username, api_key)


# Initialize a service e.g. SMS
sms = africastalking.SMS


# Use the service synchronously
response = sms.send("Hello Message!", ["2547xxxxxx"])

# Or use it asynchronously
def on_finish(error, response):
    if error is not None:
        raise error
    print(response)

sms.send("Hello Message!", ["2547xxxxxx"], callback=on_finish)    

```

See [example](example/) for more usage examples.


## Initialization

The following static methods are available in the `africastalking` module to initialize the library:

- `initialize(username, api_key)`: Initialize the library.

- `XXX`: Get an instance to `XXX` service. e.g. `africastalking.Airtime` for the airtime service

## Services

All methods are synchronous (i.e. will block current thread) but provide asynchronous variants that take a callback `(error: AfricasTalkingException, data: dict)`.
The synchronous variant always return and instance a `dict` while the async one returns a `Thread`.

### `AccountService`

- `fetch_account()`: Get app balance info.

### `AirtimeService`

- `send(phone_number, amount)`: Send airtime to a phone number. Example amount would be `KES 150`.

- `send(recipients)`: Send airtime to a bunch of phone numbers. The keys in the `recipients` map are phone numbers while the values are airtime amounts. The amounts need to have currency info e.g. `UXG 4265`.

For more information about status notification, please read [http://docs.africastalking.com/airtime/callback](http://docs.africastalking.com/airtime/callback)


### `SmsService`

- `send(message, recipients, sender_id = None, enqueue = False)`: Send a bulk message to recipients, optionally from sender_id (Short Code or Alphanumeric).

- `send_premium(message, keyword, link_id, recipients)`: Send a premium SMS

- `fetch_messages(last_received_id = 0)`: Fetch your messages

- `fetch_subscriptions(short_code, keyword, last_received_id = 0)`: Fetch your premium subscription data

- `create_subscription(short_code, keyword, phone_number, checkout_token)`: Create a premium subscription

For more information on: 

- How to receive SMS: [http://docs.africastalking.com/sms/callback](http://docs.africastalking.com/sms/callback)
- How to get notified of delivery reports: [http://docs.africastalking.com/sms/deliveryreports](http://docs.africastalking.com/sms/deliveryreports)
- How to listen for subscription notifications: [http://docs.africastalking.com/subscriptions/callback](http://docs.africastalking.com/subscriptions/callback)

### `PaymentService`

- `card_checkout(product_name, amount, payment_card, narration)`: Initiate card checkout.

- `validate_card_checkout(transaction_id, otp)`: Validate a card checkout

- `bank_checkout(product_name, amount, bank_account, narration)`: Initiate bank checkout.

- `validate_bank_checkout(transaction_id, otp)`: Validate a bank checkout

- `bank_transfer(product_name, recipients)`: Move money form payment wallet to bank account

- `mobile_checkout(product_name, phone_number, amount)`: Initiate mobile checkout.

- `mobile_b2c(product_name, consumers)`: Send mobile money to consumer. 

- `mobile_b2b(product_name, recipient)`: Send mobile money to business.


For more information, please read [http://docs.africastalking.com/payments](http://docs.africastalking.com/payments)

### `VoiceService`

- `call(phone_number)`: Initiate a phone call

- `fetch_queued_calls(phone_number)`: Get queued calls

- `upload_media_file(phone_number, url)`: Upload voice media file


For more information, please read [http://docs.africastalking.com/voice](http://docs.africastalking.com/voice)


### `TokenService`

- `create_checkout_token(phone_number)`: Create a new checkout token for `phone_number`.

- `generate_auth_token()`: Generate an auth token to use for authentication instead of an API key.

### `UssdService` *TODO?*

For more information, please read [http://docs.africastalking.com/ussd](http://docs.africastalking.com/ussd)


## Development
```shell
$ git clone https://github.com/aksalj/africastalking-python.git
$ cd africastalking-python
$ touch .env
```

Make sure your `.env` file has the following content then run `python -m unittest discover -v`

```ini
# AT API
USERNAME=sandbox
API_KEY=some_key
```

## Issues

If you find a bug, please file an issue on [our issue tracker on GitHub](https://github.com/AfricasTalkingLtd/africastalking-python/issues).
