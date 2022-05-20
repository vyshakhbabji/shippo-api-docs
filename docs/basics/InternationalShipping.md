International Shipments
=======================

You can easily handle foreign addresses and customs forms directly through the Shippo API when shipping internationally (this includes shipping to US Territories for USPS). We've created a guide to make sure your packages clear customs and arrive safely!

1\. Create the Sender & Recipient Address Objects and Parcel Object
-------------------------------------------------------------------

As with every Shipment, you need to first create or retrieve your two Address (sender and recipient) and Parcel objects first. Check out our [address validation tutorial](https://goshippo.com/docs/address-validation#global-address-validation) on how you can validate global addresses.

The only special requirement in this case is that the phone number of the sender and recipient are required. Apart from that there's nothing special about them for international shipments.

2\. Create a Customs Declaration and Customs Items
--------------------------------------------------

You will need to create a customs declaration and specify the items that are inside your international shipment. This is to ensure that the country you import goods into accepts your package at the border. The USPS website has a great [Shipping Restrictions](https://www.usps.com/ship/shipping-restrictions.htm) page with some general no-goes, as well as a more detailed [Index of Countries and Localities.](http://pe.usps.com/text/Imm/immctry.htm)

What Information Do I Need to Submit for a Customs Declaration?
---------------------------------------------------------------

You can find an overview of all available fields in our [Customs Declaration reference section](https://goshippo.com/docs/reference#customs-declarations). Most carriers require you to specify `certify`, `certify_signer`, `contents_type`, `eel_pfc` and `incoterm`. Although not all carriers require it, we also recommend submitting the `items` field -- this significantly reduces the risk of the package being stuck in customs.

There are many optional fields that can be filled out depending on who you're shipping with, so please do your research ahead of time to make sure that you have all the required fields covered.

Create Your Customs Declaration and Items Inline
------------------------------------------------

To create a customs declaration, send a POST request with the necessary information to the Customs Declarations endpoint. You can create your Customs Items within the `items` attribute. Alternatively, you can create customs items individually via the [Customs Items API endpoint](https://goshippo.com/docs/reference#customs-items).

cURL

Ruby

Python

PHP

Node

Java

C#

```
curl https://api.goshippo.com/customs/declarations/\
    -H "Authorization: ShippoToken <API_TOKEN>"\
    -H "Content-Type: application/json"\
    -d '{
          "contents_type": "MERCHANDISE",
          "non_delivery_option": "RETURN",
          "certify": true,
          "certify_signer": "Simon Kreuz",
          "incoterm": "DDU",
          "items": [{
                    "description": "T-shirt",
                    "quantity": 20,
                    "net_weight": "5",
                    "mass_unit": "lb",
                    "value_amount": "200",
                    "value_currency": "USD",
                    "tariff_number": "",
                    "origin_country": "US"
            }]
    }'
```

3\. Create the Shipment, Get Rates, and Purchase Label
------------------------------------------------------

The only difference between this Shipment request and a domestic Shipment request is that you must include customs declaration in the Shipment API call. After that, you can retrieve Rates and create labels [just like for domestic shipments.](https://goshippo.com/docs/shipping-labels/)

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
    -d address_from="d799c2679e644279b59fe661ac8fa488"\
    -d address_to="42236bcf36214f62bcc6d7f12f02a849"\
    -d parcels=["7df2ecf8b4224763ab7c71fae7ec8274"]\
    -d customs_declaration="b741b99f95e841639b54272834bc478c"\
    -d async=false
```

You will get a response with a `rates` attribute. The `rates` usually contains multiple rates -- use your own business logic to filter out the rate you want to use and grab the corresponding Rate's object_id.

```
{
   "object_created":"2014-07-17T00:04:06.163Z",
   "object_updated":"2014-07-17T00:04:06.163Z",
   "object_id":"89436997a794439ab47999701e60392e",
   "object_owner":"shippotle@goshippo.com",
   "status":"SUCCESS",
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
        "street1":"200 University Ave W",
        "city":"Waterloo",
        "state":"ON",
        "zip":"N2L 3G1",
        "country":"CA",
        "phone":"+1 555 341 9393",
        "email":"support@goshippo.com",
        "is_residential": false
    },
   "address_return":null,
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
   "shipment_date":"2013-12-03T12:00:00Z",
   "extra":{
      "insurance": {
         "currency": "",
         "amount": "",
      },
      "reference_1": "",
      "reference_2": ""
   },
   "customs_declaration":"b741b99f95e841639b54272834bc478c",
   "rates": [
        {
            "object_created": "2014-07-17T00:04:06.263Z",
            "object_id": "545ab0a1a6ea4c9f9adb2512a57d6d8b",
            "object_owner": "shippotle@goshippo.com",
            "shipment": "89436997a794439ab47999701e60392e",
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
               "token": "usps_priority",
               "terms": ""
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
   "messages":[],
   "metadata":"Customer ID 123456"
}
```

POST to the Transaction endpoint with your Rate object_id to purchase your international shipping label.

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

You will receive a JSON serialized Transaction object with your label, commercial invoice, and tracking information.

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

#### What about Commercial Invoices?

Most international shipments require you to add 3 commercial invoices in the package's "pouch", a special envelope attached on the package. Shippo automatically creates these 3 copies for you, which will be returned in the Transaction's `commercial_invoice` field.