Authentication
==============

Before you can start using the Shippo API, you'll need to register for a free Shippo account and get your API live token from the [API page](https://apps.goshippo.com/login?next=/user/apikeys/) of the dashboard.

To create production shipping labels and make live requests, you must authenticate your requests using the live token. All code samples in these tutorials use your test token.

cURL

Ruby

Python

PHP

Node

Java

C#

```
curl https://api.goshippo.com/shipments/\
    -H "Authorization: ShippoToken <API_TOKEN>"\
    -H "Content-Type: application/json"\
    -d '{...}'
```

### Client Libraries

To make your integration process go faster, we've created client libraries in Ruby, PHP, Python, NodeJS, Java and C#. We recommend installing them first before you start with Shippo.

[Learn more about client libraries here ▸](https://goshippo.com/docs/client-libraries)

### Test Mode

You can try out the Shippo API and get test labels for free by using your API test token instead of the live token.

[Learn more about test mode ▸](https://goshippo.com/docs/test-mode)