Test Mode
=========

Test mode behavior may differ if you are using Shippo API version 2014-02-11. To see what version you are currently running [go to your dashboard.](https://apps.goshippo.com/login?next=/user/apikeys/) To learn more about API version, [head here.](https://goshippo.com/docs/versioning)

To use the Shippo API end-to-end and create test shipping labels for free, use your test token to set your account to test mode.

### Accessing Test Mode

You can find your Shippo test token from the [API section of your dashboard.](https://apps.goshippo.com/login?next=/user/apikeys/)

To turn on test mode, insert your Shippo API test token in place of the live token. Now you can use Shippo as you would in production and be sure that you won't get charged.

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

Certain carriers require a different test account credential to show test rates and generate test labels. For more information about which carriers require a separate test account, [see our carrier capabilities page.](https://goshippo.com/docs/carriers)

### Notes About Test Mode

-   Test labels cannot be used to ship a parcel.
-   Rates requested in test mode may differ from actual rates in live mode.
-   Test tracking numbers will be generated, but tracking information will not be updated.
-   Test mode currently does not work with the batch label process and manifesting. We are actively working on supporting these features.
-   Refund requests in test mode will always return a `success` response, but no invoice item will be generated.
-   When using the test token, you will only be able to access test data -- not live data (carrier accounts, transactions, rates etc.).
-   When using the live token, you will only be able to access live data -- not test data.
    -   Exception: If you have not upgraded your API and are continuing to use version 2014-02-11, your live token will remain unchanged and you will continue to see both test data and live data. This means that your Shipment object can return one of 3 different values for the variables `test`
        -   True -- All Rates returned in Shipment were tests
        -   False -- All Rates returned in Shipment were live
        -   None -- Rates returned in Shipment were both tests and live
-   User-owned carrier accounts (where you have plugged in your own carrier credentials) created in with test tokens will only be available in test mode. To create live labels, users will need to connect their carrier accounts again using their live token. Similarly, carrier accounts created using the live token will not work in test mode.
    -   Shippo-owned master accounts will not require this, the carrier account object_ids will work for both test and live mode.
    -   Exception: If you have not upgraded your API and are continuing to use version 2014-02-11, you will not need to create separate test and live carrier accounts to view test data and create test labels when in live mode.