Multi-Piece Shipment
====================

Shipments with multiple packages sent to the same destination can be grouped together in a multi-piece shipment to save money.

Multi-piece packages/parcels in a single shipment may receive timeouts at certain thresholds based on the carrier. Currently requesting rates for 5 or more packages/parcels in a single shipment may result in timeouts for UPS. Additionally, sending more than 12 parcels within one shipment for FedEx may result in a timeout. USPS does not support multi-piece packages/parcels in a single shipment at this time.

How to Create a Multi-Piece Shipment
------------------------------------

To create a multi-piece shipment, simply add a list of Parcel objects to the `parcels` field in a Shipment. Generating rates and creating transactions works the exact same way as with normal shipments. The limit on the number of parcels depends on each carrier's restrictions.

The Shipment request content should look like:

cURL

Ruby

Python

PHP

Node

Java

C#


```json
curl https://api.goshippo.com/shipments/\
    -H "Authorization: ShippoToken <API_Token>"\
    -H "Content-Type: application/json"\
    -d '{
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
            "parcels": [
                {
                    "length": "5",
                    "width": "5",
                    "height": "5",
                    "distance_unit": "in",
                    "weight": "2",
                    "mass_unit": "lb"
                },
                {
                    "length": "10",
                    "width": "10",
                    "height": "10",
                    "distance_unit": "in",
                    "weight": "2",
                    "mass_unit": "lb"
                }
            ],
            "async": false
        }'

```

As usual, the Shipment request will return a list of Rates for you to select from. The Rate `amount` refers to the cost of the entire Shipment with multiple packages, not per package.


```json
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
        "phone": "+15553419393",
        "email": "support@goshippo.com",
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
        "phone":"+1 555 341 9393",
        "email":"support@goshippo.com",
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
        "phone": "+15553419393",
        "email": "support@goshippo.com",
        "is_residential": null
    },
    "parcels": [
        {
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
        },
        {
            "object_id": "dbe0260bb2674c709fa45667cf353f27",
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
        }
    ],
    "shipment_date": "2013-12-03T12:00:00.000Z",
    "extra": {
        "insurance": {
            "amount": "",
            "currency": ""
        },
        "reference_1": "",
        "reference_2": ""
    },
    "customs_declaration": "",
    "rates": [
        {
            "object_created": "2013-12-01T06:24:21.121Z",
            "object_id": "545ab0a1a6ea4c9f9adb2512a57d6d8b",
            "object_owner": "shippotle@goshippo.com",
            "shipment": "5e40ead7cffe4cc1ad45108696162e42",
            "attributes": [],
            "amount": "65.80",
            "currency": "USD",
            "amount_local": "65.80",
            "currency_local": "USD",
            "provider": "USPS",
            "provider_image_75": "https://cdn2.goshippo.com/providers/75/USPS.png",
            "provider_image_200": "https://cdn2.goshippo.com/providers/200/USPS.png",
            "servicelevel": {
                "name": "Ground",
                "token": "fedex_ground",
                "terms": ""
            },
            "days": 5,
            "arrives_by": null,
            "duration_terms": null,
            "messages": [],
            "carrier_account": "078870331023437cb917f5187429b093",
            "test": false
        },
        ...
    ],
    "carrier_accounts": [],
    "metadata": "Customer ID 123456",
    "messages": []
}
```


How to Retrieve Labels for Each Package
---------------------------------------

You can purchase a multi-piece shipping rate by POSTing the Rate `object_id` to the Transaction endpoint as usual. A separate transaction will be created for each parcel.

cURL

Ruby

Python

PHP

Node

Java

C#

```
curl https://api.goshippo.com/transactions\
    -H "Authorization: ShippoToken <API_Token>"\
    -d rate="cf6fea899f1848b494d9568e8266e076"
    -d label_file_type="PDF"
    -d async=false
```

You can also create multi-piece shipments in one API call. Check out our [tutorial for single label call creation](https://goshippo.com/docs/single-call) for more details.

The POST request returns the master Transaction object in the response. In order to get labels for all the parcels in the Shipment you need to filter for each parcel transaction.

1.  The Transaction response after you've purchased the rate provides you with the master `tracking_number`of the entire Shipment and the `label_url` for the first parcel of the Shipment.\
    As an example, [this label](https://shippo-static.s3.amazonaws.com/img/illustrations/mps1.png) has the master tracking number on it incl. information about the status of the entire multi-piece shipment.
2.  To retrieve labels for the rest of the parcels in the Shipment, you need to make another GET request to the Transaction endpoint with the Rate `object_id` as the query parameter.\
    [These sample labels](https://shippo-static.s3.amazonaws.com/img/illustrations/mps2.png) have both the master tracking number on it as well as their own parcel-specific tracking number.

Here's an example request:

cURL

Ruby

Python

PHP

Node

```
curl https://api.goshippo.com/transactions/?rate=cf6fea899f1848b494d9568e8266e076\
-H "Authorization: ShippoToken <API_Token>"
```

The API will respond with a JSON serialized list of the transactions that belong to this Rate. Each of these transactions belongs to exactly one parcel of the multi-piece shipment. Each transaction has a unique `tracking_number` and `label_url` field for it's associated parcel.

```
{
   "count":5,
   "next":null,
   "previous":null,
   "results":[
      {
         "object_state":"VALID",
         "status":"SUCCESS",
         "object_created":"2014-07-17T00:43:40.842Z",
         "object_updated":"2014-07-17T00:43:50.531Z",
         "object_id":"70ae8117ee1749e393f249d5b77c45e0",
         "object_owner":"shippotle@goshippo.com",
         "was_test":true,
         "rate":"ee81fab0372e419ab52245c8952ccaeb",
         "tracking_number":"9499907123456123456781",
         "tracking_status":{
            "object_created":"2014-07-17T00:43:50.402Z",
            "object_id":"907d5e6120ed491ea27d4f681a7ccd4d",
            "status":"UNKNOWN",
            "status_details":"",
            "status_date":null
         },
         "tracking_url_provider":"https://tools.usps.com/go/TrackConfirmAction_input?origTrackNum=9499907123456123456781",
         "eta":"2014-07-21T12:00:00.000Z",
         "label_url":"https://shippo-delivery.s3.amazonaws.com/70ae8117ee1749e393f249d5b77c45e0.pdf?Signature=vDw1ltcyGveVR1OQoUDdzC43BY8%3D&Expires=1437093830&AWSAccessKeyId=AKIAJTHP3LLFMYAWALIA",
         "commercial_invoice_url": "",
         "messages":[

         ],
         "metadata":""
      },
      {...},
      {...}
   ]
}
```

If you have more than 5 transactions that belong to the Rate, the response will be paginated.

You can modify your request to increase the number of transactions per page. Example:

```
https://api.goshippo.com/transactions/?results=10&rate=ee81fab0372e419ab52245c8952ccaeb
```

Or you can retrieve each page of the response listed in the 'next' parameter. Example:

```
"next": https://api.goshippo.com/transactions/?rate=ee81fab0372e419ab52245c8952ccaeb&page=2
```

Parcel-level Insurance
----------------------

You can also purchase insurance for each package of the multi-piece shipment. This can be done through the [extras](https://goshippo.com/docs/reference#parcels-extras) attribute when creating your Parcel object within the Shipment request. Here is a sample request for a Shipment with different insurance amounts for each Parcel.

```
curl https://api.goshippo.com/shipment/\
-H "Authorization: ShippoToken <API_Token>"\
-H "Content-Type: application/json"\
-d '{
    "address_from": "d799c2679e644279b59fe661ac8fa488",
    "address_to": "42236bcf36214f62bcc6d7f12f02a849",
    "parcels": [
        {
            "length": "5",
            "width": "5",
            "height": "5",
            "distance_unit": "cm",
            "weight": "2",
            "mass_unit": "lb",
            "template": "",
            "metadata": "Box1",
            "extra": {
                "insurance": {
                    "amount": 25.00,
                    "currency": "USD",
                    "provider": "FEDEX"
                    }
            }
        },
        {
            "length": "5",
            "width": "10",
            "height": "15",
            "distance_unit": "cm",
            "weight": "6",
            "mass_unit": "lb",
            "template": "",
            "metadata": "Box2",
            "extra": {
                "insurance": {
                    "amount": 50.00,
                    "currency": "USD",
                    "provider": "FEDEX"
                    }
            }
        },
        {
            "length": "2",
            "width": "8",
            "height": "9",
            "distance_unit": "cm",
            "weight": "5",
            "mass_unit": "lb",
            "template": "",
            "metadata": "Box3",
            "extra": {
                "insurance": {
                    "amount": 45.00,
                    "currency": "USD",
                    "provider": "FEDEX"
                    }
            }
        }
    ],
    "async": false
}
```

### Handling Shipment-level and Parcel-level Insurance

-   If both shipment-level and parcel-level insurance are specified, the parcel-level insurance will take precedence. Parcels without parcel-level insurance will have the shipment-level insurance amount applied to that parcel.
-   If only shipment-level insurance is specified, then the shipment insurance amount will be applied to *each parcel* -- not divide amongst parcels.

Parcel-level Cash on Delivery
-----------------------------

FedEx and UPS offer Cash on Delivery (C.O.D.) for individual parcels in a multi-piece shipment. You can specific your C.O.D. preference in the [extras](https://goshippo.com/docs/reference#parcels-extras) attribute of the Parcel object when creating your Shipment.

C.O.D. is only available for FedEx Ground as FedEx Ground packages can be delivered at different times. FedEx Express shipments can only accept C.O.D. on a shipment-level, which can be added under [extras attribute of the Shipment object.](https://goshippo.com/docs/reference#shipments-extras)