Batch Label Creation
====================

The Batch endpoint allows you to create and purchase up to 10,000 shipments in a single API request.

There are three main steps to the Batch workflow:

1.  [Create the batch](https://goshippo.com/docs/batch#create-batch): POST a list of shipments to be purchased with one API call
2.  [Fix validation issues, if any](https://goshippo.com/docs/batch#fix-batch): after creating a batch, Shippo validates the shipment data. Any shipments that failed validation need to be fixed
3.  [Purchase the batch](https://goshippo.com/docs/batch#purchase-batch): purchase labels for all shipments in the batch with one API call

Step 1: Creating the Batch
--------------------------

The Batch Object contains an array of BatchShipment. The [BatchShipment Object](https://goshippo.com/docs/reference#batches-batch-shipments) acts as a wrapper around the Shipment Object, containing shipment-specific service level information such as carrier and service level.

To create a batch, send a JSON request to the [Batch endpoint](https://goshippo.com/docs/reference#batches) with an array of BatchShipment objects.

The following is an example of the request.

```
{
    "batch_shipments": [
        {
            "servicelevel_token": "usps_priority_express",
            "shipment": {
                "address_from": "d2ce085dd3734a22b20c6df36a63aa07",
                "address_to": "8172f0a35d6d4ff6a37e7a082e4da7a6",
                "parcels": [
                    "f93c159892f54402bf14a50488ca2c36"
                ]
            }
        },
        {
            "carrier_account": "a4391cd4ab974f478f55dc08b5c8e3b3",
            "servicelevel_token": "fedex_2_day",
            "shipment": {
                "address_from": "d2ce085dd3734a22b20c6df36a63bb07",
                "address_to": "8172f0a35d6d4ff6a37e7a082e4da7b6",
                "parcels": [
                    "f93c159892f54402bf14a50488ca2c38"
                ]
            }
        }
    ],
    "default_carrier_account": "33391cd4ab974f478f55dc08b5c8e3b3",
    "default_servicelevel_token": "usps_priority",
    "label_filetype": "PDF_4x6",
    "metadata": "BATCH #170"
}
```

[This request is done asynchronously.](https://goshippo.com/docs/async) An empty Batch object is returned first while BatchShipments are created in the background. Since creating many Shipments can take some time, asynchronous requests will let you process other tasks in the meantime.

You can opt to be notified when the Shipment creation is complete through a webhook or polling. You can set up your webhooks from [your Shippo dashboard](https://apps.goshippo.com/login?next=/user/apikeys/), by setting the event type to `batch_created`. For more information, see our [Webhooks tutorial.](https://goshippo.com/docs/webhooks)

Here is a sample of the empty response.

```
{
    "batch_shipments": {
        "count": 0,
        "next": null,
        "previous": null,
        "results": []
    },
    "default_carrier_account": "bf8ef2b8aba24ac0b2efde9c72349919",
    "default_servicelevel_token": "usps_priority",
    "label_filetype": "PDF_4x6",
    "label_url": [],
    "metadata": "BATCH #170",
    "object_created": "2016-08-09T16:52:47.626Z",
    "object_id": "c6937c15a99440758b75cde7f18e2a0d",
    "object_owner": "support@goshippo.com",
    "object_results": {
        "creation_failed": 0,
        "creation_succeeded": 0,
        "purchase_failed": 0,
        "purchase_succeeded": 0
    },
    "object_updated": "2016-08-09T16:52:47.626Z",
    "status": "VALIDATING"
}
```

Default service levels vs. per-shipment service levels
------------------------------------------------------

When creating your Batch, you can choose to apply one carrier service level for all your Shipments, or specify the service level on a per-shipment bases. [Please see our service level tokens in the API reference guide.](https://goshippo.com/docs/reference#servicelevels)

If you want to set one service levels for all the Shipments in your batch, you can specify the service level token with `default_servicelevel_token` field in your Batch request.

This field can be overridden on a per-shipment basis by filling out the optional `servicelevel_token` field on the BatchShipment object.

The following is an example of this behavior:

```
{
    "batch_shipments": [
        {
            "carrier_account": "a4391cd4ab974f478f55dc08b5c8e3b3",
            "servicelevel_token": "fedex_2_day",
            "shipment": {
                "address_from": "d2ce085dd3734a22b20c6df36a63aa07",
                "address_to": "8172f0a35d6d4ff6a37e7a082e4da7a6",
                "parcels": [
                    "f93c159892f54402bf14a50488ca2c36"
                ]
            }
        }
    ],
    "default_carrier_account": "33391cd4ab974f478f55dc08b5c8e3b3",
    "default_servicelevel_token": "usps_priority",
    "label_filetype": "PDF_4x6",
    "metadata": "BATCH #170"
}
```

Step 2: Fixing Validation Issues
--------------------------------

Once the Batch Object is created, the status will change from VALIDATING to either VALID or INVALID. If the status is INVALID, that means some BatchShipments in the Batch have errors that need to be fixed before you can purchase labels.

To find invalid BatchShipments in the Batch, set the `object_results` query parameter on the Batch endpoint to filter by Shipments that have failed. The following is an example of using the `object_results` query parameter:

```
curl https://api.goshippo.com/batches/<BATCH_OBJECT_ID>?page=2&object_results=creation_failed\
-H "Authorization: ShippoToken <PRIVATE_TOKEN>"
```

The `object_results` query parameter accepts the following values:

-   creation_failed
-   creation_succeeded
-   purchase_succeeded
-   purchase_failed

You can remove invalid BatchShipment objects by sending a PUT request with the invalid BatchShipment object IDs to the following endpoint:

```
https://api.goshippo.com/batches/<BATCH OBJECT ID>/remove_shipments
```

Here is what the sample request should look like:

```
[
    "aa7dea463a5a48b0b8fb21f90e72d779",
    "f11b46440c144ce3b97fb5ddf02b8c71",
    "5400f9078f764b1bbb121bcd08de127f",
    "2ab2b452392545908d2cef8861a39c35"
]
```

Or you can add fixed or new shipments to the batch by passing a POST request with a list of BatchShipment objects to the endpoint:

```
https://api.goshippo.com/batches/<BATCH OBJECT ID>/add_shipments
```

Here's a sample of what the request payload should look like:

```
[
    {
        "carrier_account": "a4391cd4ab974f478f55dc08b5c8e3b3",
        "servicelevel_token": "fedex_2_day",
        "shipment": {
            "address_from": "d2ce085dd3734a22b20c6df36a63aa07",
            "address_to": "8172f0a35d6d4ff6a37e7a082e4da7a6",
            "parcels": [
                "f93c159892f54402bf14a50488ca2c36"
            ]
        }
    },
    {
        "carrier_account": "33391cd4ab974f478f55dc08b5c8e3b3",
        "servicelevel_token": "usps_priority_express",
        "shipment": {
            "address_from": "d2ce085dd3734a22b20c6df36a63aa07",
            "address_to": "4f406a13253945a8bc8deb0f8266b245",
            "parcels": [
                "ec952343dd4843c39b42aca620471fd5"
            ]
        }
    }
]
```

Once all shipments have been fixed, identified by the VALID Batch status, you can then proceed to purchase the Batch.

Step 3: Purchasing the Batch
----------------------------

To purchase the batch, simply send an empty POST request to the endpoint:

```
https://api.goshippo.com/batches/<BATCH OBJECT ID>/purchase
```

Here's the purchase response:

```
{
    "batch_shipments": {
        "count": 2,
        "next": null,
        "previous": null,
        "results": [
            {
                "carrier_account": "a4391cd4ab974f478f55dc08b5c8e3b3",
                "messages": [],
                "metadata": "",
                "object_id": "40f2cf49a3464614b998cc0eb61e768d",
                "servicelevel_token": "fedex_2_day",
                "shipment": "6a2579a51e4f4e49a5eb5d9c6853bd39",
                "status": "VALID",
                "transaction": null
            },
            {
                "carrier_account": null,
                "messages": [],
                "metadata": "",
                "object_id": "b9a42cb3897c4bcda64d7cf0933645df",
                "servicelevel_token": "usps_priority_express",
                "shipment": "2f7b9705c7004396bb1e35ebcf0a3c25",
                "status": "VALID",
                "transaction": null
            }
        ]
    },
    "default_carrier_account": "bf8ef2b8aba24ac0b2efde9c72349919",
    "default_servicelevel_token": "usps_priority",
    "label_filetype": "PDF_4x6",
    "label_url": [],
    "metadata": "BATCH #170",
    "object_created": "2016-08-09T16:52:47.626Z",
    "object_id": "c6937c15a99440758b75cde7f18e2a0d",
    "object_owner": "support@goshippo.com",
    "object_results": {
        "creation_failed": 0,
        "creation_succeeded": 2,
        "purchase_failed": 0,
        "purchase_succeeded": 0
    },
    "object_updated": "2016-08-09T16:58:41.060Z",
    "status": "PURCHASING"
}
```

This step is done asynchronously as well. You can [register a webhook](https://apps.goshippo.com/login?next=/user/apikeys/) with the event type `batch_purchased` to get push notifications when your labels are ready. Again, for more information, see our [Webhooks tutorial.](https://goshippo.com/docs/webhooks)

If there are any failed transactions, you can find them directly with the `object_results=purchase_failed`query parameter.

Once your purchases are complete, you will be able to download a merged PDF file with the labels. Each PDF file will contain up to 100 labels, the `label_url` field will contain an array of PDF files.