Filtering
=========

When querying an endpoint to return multiple results back, you can use query string parameters to filter the results that you are getting back. Shippo currently supports a [results](https://goshippo.com/docs/filtering#results) parameter and some [date filtering](https://goshippo.com/docs/filtering#date-filtering) parameters (only currently supported on the Shipments endpoint).

By default, you will only get 5 results back if you do not specify a `results` query string parameter.

Results
-------

The response will be paginated for however many you had specified in the `results` query string parameter, so by default you would only see 5 per page, with a `next` field indicating if there is another page of results. A `previous` field will give any previous pages if you're not currently looking at the first page.

Example
-------

URL: `https://api.goshippo.com/carrier_accounts/?results=10`

Response:

```
{
    "next": null,
    "previous": null,
    "results": [
        {
            "carrier": "parcelforce",
            "object_id": "bef77ddbd2a5455eba8fcd4511eef645",
            "object_owner": "shippotle@goshippo.com",
            "account_id": "shippo_parcelforce_account",
            "parameters": {"expresslink_password": "******"},
            "test": true,
            "active": true,
            "is_shippo_account": true,
            "metadata":""
        },
        {
            "carrier": "usps",
            "object_id": "2da62634606540c082f3612afe95ecae",
            "object_owner": "shippotle@goshippo.com",
            "account_id": "shippo_usps_account",
            "parameters": {"is_commercial": false},
            "test": true,
            "active": true,
            "is_shippo_account": true,
            "metadata": ""
        },
        {
            "carrier": "deutsche_post",
            "object_id": "595d9cb0c0e14497bf07e75ecfec6c6d",
            "object_owner": "shippotle@goshippo.com",
            "account_id": "shippo_deutsche_post_account",
            "parameters": [],
            "test": true,
            "active": true,
            "is_shippo_account": true,
            "metadata": ""
        },
        {
            "carrier": "uber",
            "object_id": "fd7f11a74c8847778fe41fed06b7fdd4",
            "object_owner": "shippotle@goshippo.com",
            "account_id": "shippo_uber_account",
            "parameters": [],
            "test": true,
            "active": true,
            "is_shippo_account": true,
            "metadata": ""
        },
        {
            "carrier": "dhl_express",
            "object_id": "3d05ffe6076741228dfea04385050c0d",
            "object_owner": "shippotle@goshippo.com",
            "account_id": "shippo_dhlexpress_account",
            "parameters": [],
            "test": true,
            "active": true,
            "is_shippo_account": true,
            "metadata": ""
        },
        {
            "carrier": "fedex",
            "object_id": "86a7793d3788422a8169cfae9c87ed26",
            "object_owner": "shippotle@goshippo.com",
            "account_id": "86753099",
            "parameters": {
                "meter": "112233445",
                "smartpost_id": 5531
            },
            "test": true,
            "active": true,
            "is_shippo_account": false,
            "metadata": ""
        },
        {
            "carrier": "dhl_germany",
            "object_id": "0bff0a09251d478cb919ead06709d96c",
            "object_owner": "shippotle@goshippo.com",
            "account_id": "2222222222",
            "parameters": {
                "default_participation_code": "01",
                "business_customer_portal_password": "pass",
                "business_customer_portal_username": "2222222222_01",
                "tracking_password": "",
                "tracking_account": ""
            },
            "test": true,
            "active": true,
            "is_shippo_account": false,
            "metadata": ""
        },
        {
            "carrier": "fedex",
            "object_id": "8d6dbc109aaf4bb983d534f1556ddc16",
            "object_owner": "shippotle@goshippo.com",
            "account_id": "86753098",
            "parameters": {
                "meter": "112233446",
                "smartpost_id": 5531
            },
            "test": true,
            "active": true,
            "is_shippo_account": false,
            "metadata": "Test Account"
        },
        {
            "carrier": "canada_post",
            "object_id": "518402beb1f24e608af7be9d85a0f71b",
            "object_owner": "shippotle@goshippo.com",
            "account_id": "3f937e0b21ef06f7",
            "parameters": {
                "payment_method": "CreditCard",
                "api_password": "secure_p@$$w0rd",
                "contract_id": null,
                "is_platform_account": true,
                "customer_number": "0086753098"
            },
            "test": true,
            "active": true,
            "is_shippo_account": false,
            "metadata": ""
        },
        {
            "carrier": "ups",
            "object_id": "2716480a9d7b4b31837c0dacd7d481f3",
            "object_owner": "shippotle@goshippo.com",
            "account_id": "shippotle",
            "parameters": {
                "cost_center": "",
                "password": "secure_p@ssw0rd",
                "account_number": "SH1PP0",
                "usps_endorsement": null,
                "surepost": ""
            },
            "test": true,
            "active": true,
            "is_shippo_account": false,
            "metadata": ""
        }
    ]
}
```

You can see in the above response the amount of carrier accounts happen to line up with the amount we requested back, but if we requested 7 results, there would only be 7 carriers in the response with a url in the `next` field.

* * * * *

Date Filtering
--------------

When querying the [Shipments endpoint](https://goshippo.com/docs/reference#shipments-list) you can have an additional way to filter results by limiting the date range. We expect a date formatted to the ISO 8601 standard, and you can see some example below of what that looks like. You can retrieve any 90 day range (or smaller) of shipments by using the following query string parameters:

-   `object_created_gt` -- object(s) created greater than a provided date time
-   `object_created_gte` -- object(s) created greater than or equal to a provided date time
-   `object_created_lt` -- object(s) created less than a provided date time
-   `object_created_lte` -- object(s) created less than or equal to a provided date time

Date format examples:

-   `"2017-01-01"`
-   `"2017-01-01T03:30:30"` or `"2017-01-01T03:30:30.5"`
-   `"2017-01-01T03:30:30Z"`

Example
-------

URL: `https://api.goshippo.com/shipments/?results=10&object_created_gte=2017-08-01T00:00:00&object_created_lte=2017-08-31T00:00:00`

```
{
    "next": "https://api.goshippo.com/shipments/?object_created_gte=2017-08-01T00%3A00%3A00&object_created_lte=2017-08-31T00%3A00%3A00&results=10&page=2",
    "previous": null,
    "results": [
        {
            "address_from": {...},
            "address_return": {...},
            "address_to": {...},
            "carrier_accounts": [],
            "customs_declaration": null,
            "extra": [],
            "messages": [],
            "metadata": "",
            "object_created": "2017-08-03T23:21:20.884Z",
            "object_id": "4a310649f51f4b15aeeabb6457b50d04",
            "object_owner": "shippotle@goshippo.com",
            "object_updated": "2017-08-03T23:22:28.553Z",
            "parcels": [...],
            "rates": [...],
            "shipment_date": "2017-08-03T23:22:28.553Z",
            "status": "SUCCESS",
            "test": true
        },
        {
            "address_from": {...},
            "address_return": {...},
            "address_to": {...},
            "carrier_accounts": [],
            "customs_declaration": null,
            "extra": [],
            "messages": [],
            "metadata": "",
            "object_created": "2017-08-05T21:11:46.431Z",
            "object_id": "76055268f88c46e8b337e2058bc61186",
            "object_owner": "shippotle@goshippo.com",
            "object_updated": "2017-08-05T21:11:46.431Z",
            "parcels": [...],
            "rates": [...],
            "shipment_date": "2017-08-05T21:11:46.431Z",
            "status": "SUCCESS",
            "test": true
        }
    ]
}
```

The above response includes all shipments created on or after August 1, 2017 up to and including August 31, 2017. If `object_created_gt` and `object_created_lt` were used, it would only retrieve shipments *between*those dates. You can use any combination of `object_created_gt`/`object_created_gte` and `object_created_lt`/`object_created_lte` to filter how you need. You just need to be sure that the date range is 90 days or less.