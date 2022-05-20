Orders
======

The orders endpoint allows you to load orders from your system to the Shippo dashboard and to create, retrieve, list, and manage orders programmatically. You can also retrieve shipping rates, purchase labels, and track shipments for each order.

This tutorial will go through the following:

1.  [Test mode:](https://goshippo.com/docs/orders/#test-mode) how to test the orders endpoint
2.  [Creating an order:](https://goshippo.com/docs/orders/#create-order) create orders programmatically via the API
3.  [Retrieve an order:](https://goshippo.com/docs/orders/#retrieve-order) get all details of an order by retrieving it
4.  [List all orders:](https://goshippo.com/docs/orders/#list-orders) list and filter your orders
5.  [Purchase a label for an order:](https://goshippo.com/docs/orders/#create-order-label) retrieve rates, create shipping labels, and track shipments for a given order
6.  [Get a packing slip for an order:](https://goshippo.com/docs/orders/#create-order-packing-slip) retrieve a PDF with all details of the order

Test Mode
---------

There are a few things to be aware of when testing the Orders endpoint. For starters, you want to be sure that you're using your Shippo test token for authenticating your requests, to ensure that all orders are being created in test mode.

When you create a label using your Shippo test token and reference your order (see [Purchase a label for an order](https://goshippo.com/docs/orders#create-order-label)), it will transition the order's status for whatever the current status is to `"SHIPPED"`. This behavior maps to how it would work in live (either using the API *or* the Dashboard).

Creating an Order
-----------------

You can create an order with all the information about your order by sending a POST request to the orders endpoint. An order typically consists of the following information:

-   `to_address`: this is a mandatory field, populated with the recipient address
-   `from_address`: this is an optional field
-   `line_items`: a line item represents the item that the buyer has purchased as part of this order. Line items are optional
-   `shipping_` fields: these are optional parameters which describe the shipping rate that your customer chose during checkout. This is the information is displayed when you're using the Shippo dashboard to help select the right label for purchasing
-   `order_number`: custom reference number for the order
-   `order_status`: the current status of the order. You can find a [list of supported values](https://goshippo.com/docs/orders#order-order-status) below
-   `placed_at`: the date and time when the order has been placed by the buyer. This is not the date and time that the order object has been created

cURL

```
curl https://api.goshippo.com/orders/\
    -H "Authorization: ShippoToken <API_Token>"\
    -H "Content-Type: application/json"\
    -d '{
            "to_address": {
                "city": "San Francisco",
                "company": "Shippo",
                "country": "US",
                "email": "shippotle@goshippo.com",
                "name": "Mr Hippo",
                "phone": "15553419393",
                "state": "CA",
                "street1": "215 Clayton St.",
                "zip": "94117"
            },
            "line_items": [
                {
                    "quantity": 1,
                    "sku": "HM-123",
                    "title": "Hippo Magazines",
                    "total_price": "12.10",
                    "currency": "USD",
                    "weight": "0.40",
                    "weight_unit": "lb"
                }
            ],
            "placed_at": "2016-09-23T01:28:12Z",
            "order_number": "#1068",
            "order_status": "PAID",
            "shipping_cost": "12.83",
            "shipping_cost_currency": "USD",
            "shipping_method": "USPS First Class Package",
            "subtotal_price": "12.10",
            "total_price": "24.93",
            "total_tax": "0.00",
            "currency": "USD",
            "weight": "0.40",
            "weight_unit": "lb"
        }'
```

The order object created on Shippo will look like the following:

```
{
    "currency": "USD",
    "from_address": null,
    "line_items": [
        {
            "currency": "USD",
            "description": null,
            "manufacture_country": null,
            "max_delivery_time": null,
            "max_ship_time": null,
            "object_id": "abf7d5675d744b6ea9fdb6f796b28f28",
            "quantity": 1,
            "sku": "HM-123",
            "title": "Hippo Magazines",
            "total_price": "12.10",
            "variant_title": "",
            "weight": "0.40",
            "weight_unit": "lb"
        }
    ],
    "notes": null,
    "object_id": "4f2bc588e4e5446cb3f9fdb7cd5e190b",
    "object_owner": "shippotle@goshippo.com",
    "order_number": "#1068",
    "order_status": "PAID",
    "placed_at": "2016-09-23T01:28:12Z",
    "shipping_cost": "12.83",
    "shipping_cost_currency": "USD",
    "shipping_method": "USPS First Class Package",
    "shop_app": "Shippo",
    "subtotal_price": "12.10",
    "to_address": {
        "city": "San Francisco",
        "company": "Shippo",
        "country": "US",
        "email": "shippotle@goshippo.com",
        "is_complete": true,
        "is_residential": null,
        "metadata": "",
        "name": "Mr Hippo",
        "object_created": "2016-09-23T01:38:56Z",
        "object_id": "d799c2679e644279b59fe661ac8fa488",
        "object_owner": "shippotle@goshippo.com",
        "object_updated": "2016-09-23T01:38:56Z",
        "phone": "15553419393",
        "state": "CA",
        "street1": "215 Clayton St.",
        "street2": "",
        "validation_results": [],
        "zip": "94117"
    },
    "total_price": "24.93",
    "total_tax": "0.00",
    "transactions": [],
    "weight": "0.40",
    "weight_unit": "lb"
}
```

Retrieve an Order
-----------------

You can retrieve an order by sending a GET request to the orders endpoint at `/orders/<order-object-id>`. You can retrieve any order created through the API, manually through the Shippo dashboard, or orders imported through one of our shopping cart integrations.

cURL

```
curl https://api.goshippo.com/orders/4f2bc588e4e5446cb3f9fdb7cd5e190b\
	-H "Authorization: ShippoToken <API_Token>"
```

The order object returned will be displayed like the [sample order from above](https://goshippo.com/docs/orders#order-object).

List All Orders
---------------

You can list all orders created by sending a GET request to the orders endpoint. The result will be paginated.

cURL

```
curl https://api.goshippo.com/orders/\
    -H "Authorization: ShippoToken <API_Token>"
```

The request will return the paginated list like this:

```
{
    "count": 1382,
    "next": "https://api.goshippo.com/orders/?page=2",
    "previous": null,
    "results": [
        {
            "object_id": "4f2bc588e4e5446cb3f9fdb7cd5e190b",
            "object_owner": "shippotle@goshippo.com",
            "order_number": "#1068",
            "order_status": "PAID",
            "placed_at": "2016-09-23T01:28:12Z",
            "to_address": {
                "object_created": "2016-09-23T01:38:56Z",
                "object_updated": "2016-09-23T01:38:56Z",
                "object_id": "d799c2679e644279b59fe661ac8fa488",
                "object_owner": "shippotle@goshippo.com",
                "is_complete": true,
                "validation_results": {},
                "name": "Mr Hippo",
                "company": "Shippo",
                "street1": "215 Clayton St.",
                "street2": "",
                "city": "San Francisco",
                "state": "CA",
                "zip": "94117",
                "country": "US",
                "phone": "15553419393",
                "email": "shippotle@goshippo.com",
                "is_residential": null,
                "metadata": ""
            },
            "from_address": null,
            "line_items": [
                {
                    "object_id": "abf7d5675d744b6ea9fdb6f796b28f28",
                    "title": "Hippo Magazines",
                    "variant_title": "",
                    "sku": "HM-123",
                    "quantity": 1,
                    "total_price": "12.10",
                    "currency": "USD",
                    "weight": "0.40",
                    "weight_unit": "lb",
                    "manufacture_country": null,
                    "max_ship_time": null,
                    "max_delivery_time": null,
                    "description": null
                }
            ],
            "shipping_cost": "12.83",
            "shipping_cost_currency": "USD",
            "shipping_method": "USPS First Class Package",
            "shop_app": "Shippo",
            "subtotal_price": "12.10",
            "total_price": "24.93",
            "total_tax": "0.00",
            "currency": "USD",
            "transactions": [],
            "weight": "0.40",
            "weight_unit": "lb",
            "notes": null
        },
        ...
    ]
}
```

### Filter an Order List

You can also filter the order list with the following filters:

-   `shop_app`: only return orders from a specific store platform. [See list of all shop_app values below](https://goshippo.com/docs/orders#order-shop-app)
-   `start_date`: only return orders created after a specific date and time (ISO 8601 UTC format). This is based on the `placed_at` field, meaning when the order has been placed, not when the order object was created on the Shippo
-   `end_date`: similar to the start_date filter, only return orders before after a specific date and time
-   `order_status`: only return orders with a status equal to the selected value. [See list of all order_status values below](https://goshippo.com/docs/orders#order-order-status)

The follow request, for instance, will return all Shopify orders placed after 2016/10/03 and before 2016/10/10:

cURL

```
curl https://api.goshippo.com/orders/?end_date=2017-10-10T23:59:59-07:00&page=1&results=25&shop_app=Shopify&start_date=2017-10-03T23:59:59-07:00\
    -H "Authorization: ShippoToken <API_Token>"
```

Purchase a Label for an Order
-----------------------------

When creating test labels for an order, the order status will be automatically changed from whatever status it had to `"SHIPPED"`.

There are two ways to create a shipping label for an order, outlined in the following tutorials:

-   [First Shipment](https://goshippo.com/docs/shipping-labels/): this tutorial will show you how to retrieve a list of rates first, then select a rate for purchasing
-   [Single Call Label Creation](https://goshippo.com/docs/single-call): this tutorial will allow you to specify the exact carrier and service level for purchasing in one API call

When you create the Shipment, you can reference the order's to_address object via it's `object_id`.

**Make sure to reference your order's object_id in the `order` attribute of the transaction request so that the transaction is linked to your order.**

cURL

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
            "address_to": "d799c2679e644279b59fe661ac8fa488",
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
        "servicelevel_token": "usps_first",
        "order": "4f2bc588e4e5446cb3f9fdb7cd5e190b"
        }'
```

The `object_id` and `label_url` of the newly created transaction object will be reflected in the `transactions` array of the orders object.

Get a Packing Slip for an Order
-------------------------------

A packing slip summarizes all relevant information of the order and is often used for picking and packing purposes.

You can GET a packing slip for any order via the `/orders/<order-object-id>/packingslip/` endpoint. The endpoint will return the link to the packing slip PDF. Shippo will automatically create one packing slip page per label you have created.

cURL

```
curl https://api.goshippo.com/orders/4f2bc588e4e5446cb3f9fdb7cd5e190b/packingslip/\
    -H "Authorization: ShippoToken <API_Token>"
```

The request will return the packing slip link. The link expires after 24 hours.

```
{
    "created": "2017-10-11T17:55:39.172",
    "expires": "2017-10-12T17:55:39.172",
    "slip_url": "https://shippo-delivery-east.s3.amazonaws.com/packingslip_002c28cb4c5661526c8c05f3f336a5ab.pdf?Signature=Cwdox9AJ5BPdlDtVpGWHitROtj0%3D&Expires=1539280539&AWSAccessKeyId=AKIAJGLCC5MYLLWIG42A"
}
```

Order Status Values (order_status field)
----------------------------------------

The order field `order_status` indicates the current status of an order. Supported platforms include:

-   UNKNOWN: fallback in case no other value is given
-   AWAITPAY: awaiting payment by buyer
-   PAID: paid by buyer
-   REFUNDED: refunded payment to buyer
-   CANCELLED: canceled by buyer
-   PARTIALLY_FULFILLED: some, but not all of the order items have been fulfilled
-   SHIPPED: all order items have been shipped

The order status field is a Shippo controlled field, so it cannot be changed after you set its initial state in your POST request. When an label has been created for a specific order, the order's status will be automatically changed to `"SHIPPED"`.

Supported Platforms (shop_app field)
------------------------------------

The order field `shop_app` indicates which platform the order originated from. Supported platforms include:

| Amazon | Bigcommerce | CSV_Import | eBay |
| Etsy | GoDaddy | Magento | Shopify |
| Spreecommerce | StripeRelay | Weebly | WooCommerce |
| Shippo (for orders created via the Shippo API or App) | ePages |