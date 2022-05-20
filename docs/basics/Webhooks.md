Webhooks
========

Instead of fetching updates for tracking, batch labels, and transactions manually, you can use Shippo webhooks to get notified when a status changes.

Setting Up and Testing Webhooks
-------------------------------

You can set up webhooks on the [API settings page](https://app.goshippo.com/settings/api/) of your Shippo Dashboard. This page also allows you to test webhooks with a sample payload.

![](data:image/svg+xml;charset=utf-8,%3Csvg height='424' width='720' xmlns='http://www.w3.org/2000/svg' version='1.1'%3E%3C/svg%3E)

![Webhooks Sample Screenshot](https://goshippo.com/_gatsby/image/02d8bde7d650b0bf4eee4e8dcfbc4ad2/06e864c0851050930240e7436065caf6/webhooks-screenshot.png?u=https%3A%2F%2Fwordpress-652598-2128589.cloudwaysapps.com%2Fwp-content%2Fuploads%2F2018%2F05%2Fwebhooks-screenshot.png&a=w%3D180%26h%3D106%26fm%3Dpng%26q%3D70)

We also offer a Webhooks API to allow you to programmatically create, retrieve, edit and delete webhooks. You can [access the documentation here](https://goshippo.com/docs/reference). The Webhooks API is currently in beta and the endpoints might change. Please reach out to [support@goshippo.com](mailto:product@goshippo.com) if you have questions.

Webhook Event Types
-------------------

-   `transaction_created`: sent whenever a transaction is created in your account. The POST request body will contain a JSON of the [Transaction object](https://goshippo.com/docs/reference#transactions) that was created.
-   `transaction_updated`: sent whenever a transaction is updated in your account. The POST request body will contain a JSON of the [Transaction object](https://goshippo.com/docs/reference#transactions) that was updated.
-   `track_updated`: for tracking status updates. The POST request body will contain a JSON of the [Tracking object](https://goshippo.com/docs/reference#tracks). If you are interested in the types of events returned within the track_updated field, please view more details [here](https://goshippo.com/docs/tracking/#event-definitions).
    -   For API version 2017-03-29 and older, the body will contain the [Transaction object](https://goshippo.com/docs/reference#transactions) if the label was created on Shippo.
    -   Shipment statuses are polled by us from the carrier API every ~2 hours, if there are any status changes, we immediately POST updates to the webhook.
-   `batch_created`: for creating the Batch object that contain Batch Shipments. This process is done asynchronously, so first you'd get an empty [Batch object](https://goshippo.com/docs/reference#batches) back, then Batch Shipments will be created in the background.
-   `batch_purchased`: for purchasing Batch Shipments through the Batch endpoint. This request is done asynchronously as well. Once purchases are complete, you will be able to download a merged PDF containing up to 100 labels per file.

Additionally, the `Shippo-API-Version` header is included in all webhook responses to indicate what version of the API is being used.

Webhook POSTBody Example
------------------------

```
{
  "metadata": "Shippo test webhook",
  "event": "track_updated",
  "data": {
    "test": true,
    "transaction": null,
    "tracking_history": [
      {
        "location": {
          "country": "US",
          "zip": "94103",
          "state": "CA",
          "city": "San Francisco"
        },
        "substatus": null,
        "status_date": "2020-03-08T01:29:06.473Z",
        "status_details": "The carrier has received the electronic shipment information.",
        "status": "UNKNOWN",
        "object_id": "7d7dc41675384da5b0b6d17545fb39d2",
        "object_updated": null,
        "object_created": "2020-03-12T09:49:06.473Z"
      },
      {
        "location": {
          "country": "US",
          "zip": "94103",
          "state": "CA",
          "city": "San Francisco"
        },
        "substatus": null,
        "status_date": "2020-03-09T03:34:06.473Z",
        "status_details": "Your shipment has departed from the origin.",
        "status": "TRANSIT",
        "object_id": "8bbc3b13964e4ac2b0a2e8b69297e98a",
        "object_updated": null,
        "object_created": "2020-03-12T09:49:06.473Z"
      },
      {
        "location": {
          "country": "US",
          "zip": "37501",
          "state": "TN",
          "city": "Memphis"
        },
        "substatus": null,
        "status_date": "2020-03-10T05:39:06.473Z",
        "status_details": "The Postal Service has identified a problem with the processing of this item and you should contact support to get further information.",
        "status": "FAILURE",
        "object_id": "334124a6c1fa4e70a94cd60e9b4e35d8",
        "object_updated": null,
        "object_created": "2020-03-12T09:49:06.473Z"
      },
      {
        "location": {
          "country": "US",
          "zip": "60611",
          "state": "IL",
          "city": "Chicago"
        },
        "substatus": null,
        "status_date": "2020-03-11T07:44:06.473Z",
        "status_details": "Your shipment has been delivered.",
        "status": "DELIVERED",
        "object_id": "a2b1d2301c294226ada274784a76cdb1",
        "object_updated": null,
        "object_created": "2020-03-12T09:49:06.473Z"
      }
    ],
    "tracking_status": {
      "location": {
        "country": "US",
        "zip": "60611",
        "state": "IL",
        "city": "Chicago"
      },
      "substatus": null,
      "status_date": "2020-03-11T07:44:06.471Z",
      "status_details": "Your shipment has been delivered.",
      "status": "DELIVERED",
      "object_id": "2d3d646335e449a8b0308f798970d045",
      "object_updated": null,
      "object_created": "2020-03-12T09:49:06.471Z"
    },
    "metadata": null,
    "messages": [],
    "carrier": null,
    "tracking_number": "SHIPPO_DELIVERED",
    "address_from": {
      "country": "US",
      "zip": "94103",
      "state": "CA",
      "city": "San Francisco"
    },
    "address_to": {
      "country": "US",
      "zip": "60611",
      "state": "IL",
      "city": "Chicago"
    },
    "eta": "2020-03-11T09:49:06.459Z",
    "original_eta": "2020-03-10T09:49:06.459Z",
    "servicelevel": {
      "name": null,
      "token": "shippo_priority"
    }
  },
  "carrier": "shippo",
  "test": true
}
```

Expected Webhook Behavior
-------------------------

Shippo expects your Webhook endpoint to return a 2XX HTTP status code in a timely manner (three seconds or less) to indicate that the POST payload has been received. If your Webhook endpoint does not process the payload in a timely manner, Shippo will retry once. No retry is attempted upon any non-2XX response.

Debugging
---------

If you have a public-facing webserver and would like to view the test events as they come in, you can use this BASH one-liner (listening on port 8891). This snippet returns the HTTP 200 response we expect your endpoint to return in production. (Note that you do not need to have JQ installed as this is just for pretty printing the POST Body response.)

Jq formatting for webhook test listener:

`~$ while true; do { echo -e 'HTTP/1.1 200 OK\r\n'; } | nc -l 8891 | grep test | jq . ; done`

![](data:image/svg+xml;charset=utf-8,%3Csvg height='540' width='507' xmlns='http://www.w3.org/2000/svg' version='1.1'%3E%3C/svg%3E)

![Webhooks Debugging Screenshot](https://goshippo.com/_gatsby/image/e9b6689ed69f7c1deb432691d0dfab5e/ca2dd2a34f5ca2e532d5253a386a17d1/webhooks-screenshot-2.png?u=https%3A%2F%2Fwordpress-652598-2128589.cloudwaysapps.com%2Fwp-content%2Fuploads%2F2018%2F05%2Fwebhooks-screenshot-2.png&a=w%3D127%26h%3D135%26fm%3Dpng%26q%3D70)