Alcohol Shipments
=================

Shippo supports creating labels for shipping alcohol by specifying options in the `extras` field of the Shipment object.

Creating an Alcohol Shipment
----------------------------

To create a label for a shipment that will contain alcohol, you will need to specify options in the Shipment `extra` field.

cURL

```
curl https://api.goshippo.com/shipments/\
    -H "Authorization: ShippoToken <API_Token>"\
    -d address_from="d799c2679e644279b59fe661ac8fa488"\
    -d address_to="42236bcf36214f62bcc6d7f12f02a849"\
    -d parcels=["7df2ecf8b4224763ab7c71fae7ec8274"]\
    -d extra='{"alcohol": {"contains_alcohol": true, "recipient_type": "consumer"}}'\
    -d async=false
```

In the response you'll get your usual Shipment object with all of the available rates for your alcohol shipment.

```
{
    "address_from": {
        "city": "San Francisco",
        "company": "",
        "country": "US",
        "is_complete": false,
        "is_residential": null,
        "name": "Mr Hippo",
        "object_id": "b631001acb534e2bbf7cea94adfd2d00",
        "phone": "4151234567",
        "state": "CA",
        "street1": "965 Mission St",
        "street2": "APT 572",
        "street3": "",
        "street_no": "",
        "test": true,
        "validation_results": [],
        "zip": "94103"
    },
    "address_return": {
        "city": "San Francisco",
        "company": "",
        "country": "US",
        "is_complete": false,
        "is_residential": null,
        "name": "Mr Hippo",
        "object_id": "b631001acb534e2bbf7cea94adfd2d00",
        "phone": "4151234567",
        "state": "CA",
        "street1": "965 Mission St",
        "street2": "APT 572",
        "street3": "",
        "street_no": "",
        "test": true,
        "validation_results": [],
        "zip": "94103"
    },
    "address_to": {
        "city": "Brooklyn",
        "company": "",
        "country": "US",
        "is_complete": false,
        "is_residential": null,
        "name": "Billy Bob",
        "object_id": "13d8b016de1c4befbdff900cd05ad2da",
        "phone": "4151234567",
        "state": "NY",
        "street1": "206 1ST ST",
        "street2": "STE 202",
        "street3": "",
        "street_no": "",
        "test": true,
        "validation_results": [],
        "zip": "11232"
    },
    "carrier_accounts": [
        "078870331023437cb917f5187429b093"
    ],
    "customs_declaration": null,
    "extra": {
        "alcohol": {
            "contains_alcohol": true,
            "recipient_type": "licensee"
        }
    },
    "messages": [],
    "metadata": "",
    "object_created": "2017-08-03T17:54:54.006Z",
    "object_id": "5e40ead7cffe4cc1ad45108696162e42",
    "object_owner": "shippotle@goshippo.com",
    "object_updated": "2017-08-03T17:54:54.006Z",
    "parcels": [
        {
            "distance_unit": "in",
            "extra": [],
            "height": "3.0000",
            "length": "10.0000",
            "line_items": [],
            "mass_unit": "lb",
            "metadata": "",
            "object_created": "2017-08-03T17:54:53.976Z",
            "object_id": "6fb5951a0900439b8770bc1814ee5528",
            "object_owner": "shippotle@goshippo.com",
            "object_state": "VALID",
            "object_updated": "2017-08-03T17:54:54.024Z",
            "template": null,
            "test": true,
            "value_amount": null,
            "value_currency": null,
            "weight": "1.0000",
            "width": "3.0000"
        }
    ],
    "rates": [
        {
            "amount": "14.16",
            "amount_local": "14.16",
            "arrives_by": null,
            "attributes": [
                "BESTVALUE",
                "CHEAPEST",
                "FASTEST"
            ],
            "carrier_account": "078870331023437cb917f5187429b093",
            "currency": "USD",
            "currency_local": "USD",
            "duration_terms": "",
            "estimated_days": 5,
            "messages": [],
            "object_created": "2017-08-03T17:54:56.057Z",
            "object_id": "d870ae7efc7c45eabf097986d45d4f09",
            "object_owner": "shippotle@goshippo.com",
            "provider": "FedEx",
            "provider_image_200": "https://shippo-static.s3.amazonaws.com/providers/200/FedEx.png",
            "provider_image_75": "https://shippo-static.s3.amazonaws.com/providers/75/FedEx.png",
            "servicelevel": {
                "name": "Ground",
                "terms": "",
                "token": "fedex_ground"
            },
            "shipment": "6781b0124d2d4f8cbdd24bd589c61460",
            "test": true,
            "zone": "8"
        }
    ],
    "shipment_date": "2017-08-03T17:54:53.975Z",
    "status": "SUCCESS"
}
```

Create an Alcohol Shipment Label
--------------------------------

You can then use any rate `object_id` that was returned to create a transaction and get your shipping label:

cURL

```
curl https://api.goshippo.com/transactions\
    -H "Authorization: ShippoToken <API_Token>"\
    -d rate="cf6fea899f1848b494d9568e8266e076"
    -d label_file_type="PDF"
    -d async=false
```

FedEx returns two labels that *both* need to be placed on the shipment. You will only receive one `label_url` that is two pages long in the transaction object.

Your transaction response will look the same as any other shipment:

```
{
    "commercial_invoice_url": null,
    "eta": null,
    "label_url": "https://shippo-delivery-east.s3.amazonaws.com/fe52964dbfe449efb6210f0caa9fdee8.pdf?Signature=aF9ujJ%2BOV5pJc24XoTuKeBmfJCQ%3D&Expires=1385930652&AWSAccessKeyId=AKIAJTHP3LLFMYAWALIA",
    "messages": [],
    "metadata": "",
    "object_created": "2017-08-03T17:55:44.347Z",
    "object_id": "fe52964dbfe449efb6210f0caa9fdee8",
    "object_owner": "shippotle@goshippo.com",
    "object_state": "VALID",
    "object_updated": "2017-08-03T17:55:46.086Z",
    "order": null,
    "parcel": "6fb5951a0900439b8770bc1814ee5528",
    "rate": "d870ae7efc7c45eabf097986d45d4f09",
    "status": "SUCCESS",
    "test": false,
    "tracking_number": "794609767977",
    "tracking_status": "UNKNOWN",
    "tracking_url_provider": "https://www.fedex.com/apps/fedextrack/?action=track&cntry_code=us&trackingnumber=794609767977"
}
```

Supported Carriers & Services
-----------------------------

The following carriers services are supported for alcohol shipments.

**FedEx**

-   Ground
-   Home Delivery
-   Express Saver
-   2 Day
-   2 Day A.M.
-   Standard Overnight
-   Priority Overnight
-   First Overnight