Shipping Labels
===============

The Shippo API allows you to programmatically create shipping labels from any carrier. Depending on your use case, there are two different ways to create labels:

1.  [**Rating and label purchase**](https://goshippo.com/docs/shipping-labels/#rating-labels): if you don't know which carrier and service to use, or if you want to rate shop, you can first retrieve a list of available services including shipping rates and transit times. You can then select one of the available rates to buy the label.
2.  [**Label with one API call**](https://goshippo.com/docs/shipping-labels/#instalabel): if you already know which carrier and service you want to use, you can create a label with a single API call directly.

* * * * *

Rating and Label Purchase
-------------------------

To retrieve all available rates and create a shipping label based on one of the rates, you need to follow two simple steps:

1.  **Create the Shipment object**, consisting of a sender address, a recipient address, and a parcel (blue objects below). The Shipment response will contain the list of available Rates.
2.  **Create the Transaction object**, i.e. the actual label, for any of the Rates from the Shipment response (green object below).

![](https://shippo-static.s3.amazonaws.com/img/illustrations/api-object-flow.png)

#### 1\. Create the Shipment Object

The purpose of the Shipment object is to retrieve rates. It represents a request to ship a given package from the sender to the recipient address. For both addresses make sure to send the `name`, `street1`, `city`, `state` (only for US & CA), `zip` and `country` values. All US and Australian addresses are automatically validated. For more information see our [Address Validation Tutorial.](https://goshippo.com/docs/address-validation)

The Shipment object has additional properties (such as signature confirmation, 3rd party billing, COD, etc.) that you can find in our [API reference](https://goshippo.com/docs/reference#shipments-extras).

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
    -d '{
       "address_from":{
          "name":"Mr. Hippo",
          "street1":"215 Clayton St.",
          "city":"San Francisco",
          "state":"CA",
          "zip":"94117",
          "country":"US"
       },
       "address_to":{
          "name":"Mrs. Hippo",
          "street1":"965 Mission St.",
          "city":"San Francisco",
          "state":"CA",
          "zip":"94105",
          "country":"US"
       },
       "parcels":[{
          "length":"5",
          "width":"5",
          "height":"5",
          "distance_unit":"in",
          "weight":"2",
          "mass_unit":"lb"
       }],
       "async": false
    }'
```

The API will respond with the JSON serialized Shipment object:

```
{
    "status": "SUCCESS",
    "object_created": "2013-12-01T06:24:20.121Z",
    "object_updated": "2013-12-01T06:24:20.121Z",
    "object_id": "5e40ead7cffe4cc1ad45108696162e42",
    "object_owner": "shippotle@goshippo.com",
    "address_from": {
        "object_id": "0943ae4e373e4120a99c337e496dcce8",
        "validation_results": {},
        "is_complete": true,
        "company": "",
        "street_no": "",
        "name": "Mr. Hippo",
        "street1": "215 Clayton St.",
        "street2": "",
        "city": "San Francisco",
        "state": "CA",
        "zip": "94117",
        "country": "US",
        "phone": "",
        "email": "",
        "is_residential": null
    },
    "address_to": {
        "object_id": "4c7185d353764d0985a6a7825aed8ffb",
        "validation_results": {},
        "is_complete": true,
        "name":"Mrs. Hippo",
        "street1":"965 Mission St.",
        "city":"San Francisco",
        "state":"CA",
        "zip":"94105",
        "country":"US",
        "phone":"",
        "email":"",
        "is_residential": false
    },
    "address_return": {
        "object_id": "0943ae4e373e4120a99c337e496dcce8",
        "validation_results": {},
        "is_complete": true,
        "company": "",
        "street_no": "",
        "name": "Mr. Hippo",
        "street1": "215 Clayton St.",
        "street2": "",
        "city": "San Francisco",
        "state": "CA",
        "zip": "94117",
        "country": "US",
        "phone": "",
        "email": "",
        "is_residential": null
    },
    "parcels": [{
        "object_id": "ec952343dd4843c39b42aca620471fd5",
        "object_created": "2013-12-01T06:24:21.121Z",
        "object_updated": "2013-12-01T06:24:21.121Z",
        "object_owner": "shippotle@goshippo.com",
        "template": null,
        "length":"5",
        "width":"5",
        "height":"5",
        "distance_unit":"in",
        "weight":"2",
        "mass_unit":"lb",
        "value_amount": null,
        "value_currency": null,
        "metadata": "",
        "line_items": [],
        "test": true
    }],
    "shipment_date": "2013-12-03T12:00:00.000Z",
    "extra": {
        "insurance": {
            "currency": "",
            "amount": ""
        }
    },
    "customs_declaration": "",
    "rates": [
        {
            "object_created": "2013-12-01T06:24:21.121Z",
            "object_id": "cf6fea899f1848b494d9568e8266e076",
            "object_owner": "shippotle@goshippo.com",
            "shipment": "5e40ead7cffe4cc1ad45108696162e42",
            "attributes": [],
            "amount": "5.50",
            "currency": "USD",
            "amount_local": "5.50",
            "currency_local": "USD",
            "provider": "USPS",
            "provider_image_75": "https://cdn2.goshippo.com/providers/75/USPS.png",
            "provider_image_200": "https://cdn2.goshippo.com/providers/200/USPS.png",
            "servicelevel": {
                "name": "Priority Mail",
                "token":"usps_priority",
                "terms": "",
            },
            "days": 2,
            "arrives_by": null,
            "duration_terms": "Delivery in 1 to 3 business days.",
            "messages": [],
            "carrier_account": "078870331023437cb917f5187429b093",
            "test": false,
            "zone": 1
        },
        ...
    ],
    "carrier_accounts": [],
    "metadata": "Customer ID 123456",
    "messages": []
}
```

The response contains a list of rates for the Shipment. If you haven't added your own carrier accounts, you should only see USPS and DHL Express rates (for international shipments) at the moment.

Note that "SUCCESS"  `status` and all available rates are returned  because this is a synchronous API call. For asynchronous API calls please wait for "SUCCESS"  `status`to get all available rates for your shipment.\
For more information please refer to <https://goshippo.com/docs/async/>

If you want to retrieve rates from specific carriers only, you can specify a list of carrier accounts in the `carrier_account` field of your Shipment request. When this field is set, Shippo will only retrieve rates from the corresponding carriers.

#### 2\. Create the Transaction Object

The creation of a shipping label is handled by the Transaction endpoint. The Transaction object represents a shipping label purchase and is based on the shipping rate you want to purchase from step 1.

You can send an optional `label_file_type` in the transaction call. If you don't specify this value, the API will use to the default file format, which you can set on the [settings page](https://goshippo.com/user/settings/).

cURL

Ruby

Python

PHP

Node

Java

C#

```
curl https://api.goshippo.com/transactions\
    -H "Authorization: ShippoToken <API_TOKEN>"\
    -d rate="cf6fea899f1848b494d9568e8266e076"
    -d label_file_type="PDF"
    -d async=false
```

The API will respond with the JSON serialized Transaction object:

```
{
    "object_state":"VALID",
    "status":"SUCCESS",
    "object_created":"2013-12-27T19:14:48.273Z",
    "object_updated":"2013-12-27T19:14:48.273Z",
    "object_id":"64bba01845ef40d29374032599f22588",
    "object_owner":"shippotle@goshippo.com",
    "was_test":false,
    "rate":"cf6fea899f1848b494d9568e8266e076",
    "tracking_number":"ZW70QJC",
    "tracking_status":"UNKNOWN",
    "tracking_url_provider":"https://tools.usps.com/go/TrackConfirmAction.action?tLabels=ZW70QJC","eta":"2013-12-30T12:00:00.000Z",
    "label_url":"https://shippo-delivery-east.s3.amazonaws.com/773e695165f74f90b28f07450a7b4161.pdf?Signature=c%2B0eKYDgTOn6chbwpP8Ns9T0DTs%3D&Expires=1536113105&AWSAccessKeyId=AKIAJTHP3LLFMYAWILIA",
    "commercial_invoice_url":"",
    "metadata":"",
    "parcel":"603dbc611dc04170b47c376fae7b68f7"
}
```

Congrats! You've created your first shipment. You can find the shipping label in the `label_url` field of the Transaction object, and the tracking number in the `tracking_number` field, amongst other response attributes.

* * * * *

Label Purchase with one API Call
--------------------------------

If you already know what service level you'll be shipping with, you can create a shipping label in one API call through Shippo.

Single call label creation is currently only available for a select set of carriers through Shippo. To see if your carrier is supported, see our [carrier capabilities page](https://goshippo.com/docs/carriers).

Creating a label with one API call is a POST request to the Transaction endpoint with the nested shipment information, the carrier account and the service token. A sample request looks like this:

```
{
    "shipment": {
        "address_from": {
            // sender address fields
        },
        "address_to": {
            // recipient address fields
        },
        "parcels": [
            {
                // parcel fields
            }
        ],
        ... // other relevant shipment fields
    },
    "carrier_account": "<carrier-account-object-id>",
    "servicelevel_token": "<servicelevel-token>"
}
```

Here's a sample call that instantly creates and returns a shipping label:

cURL

Ruby

Python

PHP

Node

Java

C#

```
curl https://api.goshippo.com/transactions/\
    -H "Authorization: ShippoToken <API_Token>"\
    -H "Content-Type: application/json"\
    -d '{
        "shipment": {
            "address_from": {
                "name": "Mr. Hippo",
                "street1": "215 Clayton St.",
                "city": "San Francisco",
                "state": "CA",
                "zip": "94117",
                "country": "US",
                "phone": "+1 555 341 9393",
                "email": "support@goshippo.com"
            },
            "address_to": {
                "name": "Mrs. Hippo",
                "street1": "965 Mission St.",
                "city": "San Francisco",
                "state": "CA",
                "zip": "94105",
                "country": "US",
                "phone": "+1 555 341 9393",
                "email": "support@goshippo.com"
            },
            "parcels": [{
                "length": "5",
                "width": "5",
                "height": "5",
                "distance_unit": "in",
                "weight": "2",
                "mass_unit": "lb"
            }]
        },
        "carrier_account": "b741b99f95e841639b54272834bc478c",
        "servicelevel_token": "usps_priority"
        }'
```

The API will respond with the JSON serialized Transaction object. Shippo automatically creates the corresponding rate object, which you can use to retrieve the `amount` of the label.

```
{
    "object_state": "VALID",
    "status": "SUCCESS",
    "object_created": "2013-12-27T19:14:48.273Z",
    "object_updated": "2013-12-27T19:14:48.273Z",
    "object_id": "64bba01845ef40d29374032599f22588",
    "object_owner": "shippotle@goshippo.com",
    "was_test": false,
    "rate": {
        "object_id": "cf6fea899f1848b494d9568e8266e076",
        "amount": "5.50",
        "currency": "USD",
        "amount_local": "5.50",
        "currency_local": "USD",
        "provider": "USPS",
        "servicelevel_name": "Priority Mail",
        "servicelevel_token": "usps_priority",
        "carrier_account": "078870331023437cb917f5187429b093",
    },
    "tracking_number": "ZW70QJC",
    "tracking_status": {
        "object_created": "2013-12-27T23:17:41.411Z",
        "object_id": "a21b3d6831c14ceaba6730179ce6e784",
        "status": "UNKNOWN",
        "status_details": "",
        "status_date": "2013-12-28T12:04:04.214Z"
    },
    "tracking_url_provider": "https://tools.usps.com/go/TrackConfirmAction.action?tLabels=ZW70QJC",
    "eta": "2013-12-30T12:00:00.000Z",
    "label_url": "https://shippo-delivery.s3.amazonaws.com/96.pdf?Signature=PEdWrp0mFWAGwJp7FW3b%2FeA2eyY%3D&Expires=1385930652&AWSAccessKeyId=AKIAJTHP3LLFMYAWALIA",
    "commercial_invoice_url": "",
    "metadata": "",
    "messages": []
}
```

Congrats! You've created a label with a single API call. You can find the shipping label in the `label_url` field of the Transaction object, and the tracking number in the `tracking_number` field, amongst other response attributes.

Next Steps
----------

For detailed tutorials on different shipping configurations and advanced functionalities, see our navigation sidebar. Or you can browse through our [API references](https://goshippo.com/docs/reference) to explore all our supported services.

We recommend checking out the following common API endpoints and shipping features:

-   [Add carrier accounts](https://goshippo.com/docs/carrier-accounts) -- to use carriers other than our default ones, you will need to add your own account credential and connect your own carrier account.
-   [International shipping label purchases:](https://goshippo.com/docs/international/) send packages cross-border and create customs declarations, commercial invoices and submit your documentation electronically to the customs office.
-   [Refunds:](https://goshippo.com/docs/refunds/) everyone makes mistakes and sometimes your customers might have made an error when purchasing a label. Allow them to void unused labels and get their money back.
-   [Address validation:](https://goshippo.com/docs/address-validation/) prevent failed deliveries and shipping errors by automatically validating any address worldwide with Shippo's Address Validation service.
-   [Tracking:](https://goshippo.com/docs/tracking/) keep your customers, and their buyers, informed about the status of their shipments. Engage buyers by proactively sending shipment and delivery notification, and stay on top of shipment delays or exceptions.

We also recommend that you make use of [webhooks](https://goshippo.com/docs/webhooks/) to update your customers about tracking updates in realtime and reduce your development time.