Shipping Insurance
==================

> Insure your shipments programmatically via the Shippo API to avoid losing money on lost or stolen packages. You can purchase insurance via our built-in insurance service provided by [Shipsurance](http://www.shipsurance.com/), or directly from the carriers.

About Insurance
---------------

Shippo (via Shipsurance) currently offers coverage for shipments sent with APC Postal, Asendia, Canada Post, DHL eCommerce, DHL Express, FedEx, LaserShip, Newgistics, RR Donnelley, UPS, USPS, DeutschePost, DHL Germany, DHL Germany C2C, DPD Germany, GLS Deutschland, and Hermes Germany B2C.

Alternatively you can purchase insurance directly from FedEx, UPS, and Ontrac from the Shipment [extras](https://goshippo.com/docs/reference#shipments-extras) attribute. However, we recommend using Shippo's built-in insurance -- it offers a more comprehensive coverage at a lower price.

Insurance for Single Package Shipments
--------------------------------------

For shipments consisting of only one package, you can specify insurance directly from the Shipment object.

Make sure to specify the `amount` and `currency`, as well as `content` within the `insurance` attribute inside ShipmentExtras attribute. The `provider` will default to Shippo's insurance provider.

cURL

Ruby

Python

PHP

Node

Java

C#

```
curl https://api.goshippo.com/shipments/\
    -H "Authorization: ShippoToken <API_Token>"\
    -d address_from="d799c2679e644279b59fe661ac8fa488"\
    -d address_to="42236bcf36214f62bcc6d7f12f02a849"\
    -d parcels=["7df2ecf8b4224763ab7c71fae7ec8274"]\
    -d extra='{"insurance": {"amount": "200", "currency": "USD", "content": "t-shirts" }}'\
    -d async=false
```

Each valid Shipment object request will trigger a number of Rate objects that contain the Shipment insurance information as well.

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
            "amount": "200",
            "currency": "USD",
            "content": "t-shirts"
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
            "test": false
        },
        ...
    ],
    "carrier_accounts": [],
    "metadata": "Customer ID 123456",
    "messages": [],
    "test": false
}
```

You can then create the shipping label by POSTing to the Transaction endpoint as usual:

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

Insurance for Multi-piece Shipments
-----------------------------------

For multi-piece shipments you can choose to purchase specific insurance amounts for each package. This can be done through the [extras](https://goshippo.com/docs/reference#parcels-extras) attribute when creating your Parcel object within the Shipment request.

Parcel-level coverage is only available for purchase through FedEx and UPS. Shipsurance insurance can only be applied on a Shipment-level for single package shipments.

Creating a label with one API call is like one big, nested POST request to the Transaction endpoint. The structure of the content can be summarized as follows:

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

If both shipment-level and parcel-level insurance are specified, the parcel-level insurance will take precedence. Parcels without parcel-level insurance will have the shipment-level insurance amount applied to that parcel.

If only shipment-level insurance is specified, then the shipment insurance amount will be applied to each parcel and not divided amongst parcels.

### Canceling or Refunding Insurance

Please see our page on [Refunds](https://goshippo.com/docs/refunds) for more info on refunding insured shipments.