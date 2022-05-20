Tracking
========

The Shippo Tracking API allows you to track shipments across all carriers with normalized data, full tracking history and real-time updates. When combined with webhooks, you will get push-style notifications anytime a tracking update occurs from the carrier. This powerful combination allows you to always know what the latest tracking information is across all of your shipments and if a recipient may need to take action to retrieve their shipment in a single service.

This tutorial will guide you through the following:

1.  [Track live shipments:](https://goshippo.com/docs/tracking/#track-live-shipments) get live tracking data via the API.
2.  [Event definitions:](https://goshippo.com/docs/tracking/#event-definitions) get details of all possible Shippo tracking events.
3.  [Test mode:](https://goshippo.com/docs/tracking/#test-mode) how to test the tracking endpoint.
4.  [Track Shippo shipments:](https://goshippo.com/docs/tracking/#track-shippo-shipments) track shipments from labels purchased on the Shippo platform.
5.  [Track individual shipments:](https://goshippo.com/docs/tracking/#track-individual-shipments) track individual shipments without subscribing to webhooks.
6.  [Adding metadata:](https://goshippo.com/docs/tracking/#adding-metadata) free form field to add your own data.
7.  [Carrier restrictions:](https://goshippo.com/docs/tracking/#carrier-restrictions) list of carriers with limitations or restrictions explained.
8.  [Tracking pages:](https://goshippo.com/docs/tracking/#tracking-pages) setup your own tracking pages with your brand to send to customers

Track Live Shipments
--------------------

Let's get started tracking live shipments using the Shippo Tracking API in two easy steps. You can track shipments with Shippo using just a tracking number and the name of the carrier:

1.  Setup a [webhook](https://goshippo.com/docs/webhooks).
2.  POST the below to [https://api.goshippo.com/tracks/](https://goshippo.com/docs/reference#tracks-create)

```
{
    "carrier": "usps",
    "tracking_number": "9102969010383081813033"
}
```

It's important to know that you must have a webhook registered with us prior to POSTing to the tracking endpoint, because once POSTed, all updates to that tracking number will only be sent to your active tracking webhooks.

You can find a [list of all supported carrier tokens here.](https://goshippo.com/docs/reference#carriers) Also, certain carriers require an account to track shipments sent through them, please see [Carrier Restrictions](https://goshippo.com/docs/tracking#carrier-restrictions) below for details.

You will get a response back that includes the latest tracking status via the corresponding `tracking_status` field, and the entire history via the `tracking_history` field, see a sample response below.

```
{
  "carrier": "usps",
  "tracking_number": "9205590164917312751089",
  "address_from": {
    "city": "Las Vegas",
    "state": "NV",
    "zip": "89101",
    "country": "US"
  },
  "address_to": {
    "city": "Spotsylvania",
    "state": "VA",
    "zip": "22551",
    "country": "US"
  },
  "transaction": "1275c67d754f45bf9d6e4d7a3e205314",
  "original_eta": "2016-07-23T00:00:00Z",
  "eta": "2016-07-23T00:00:00Z",
  "servicelevel": {
    "token": "usps_priority",
    "name": "Priority Mail"
  },
  "metadata": null,
  "tracking_status": {
    "object_created": "2016-07-23T20:35:26.129Z",
    "object_updated": "2016-07-23T20:35:26.129Z",
    "object_id": "ce48ff3d52a34e91b77aa98370182624",
    "status": "DELIVERED",
    "status_details": "Your shipment has been delivered at the destination mailbox.",
    "status_date": "2016-07-23T13:03:00Z",
    "location": {
      "city": "Spotsylvania",
      "state": "VA",
      "zip": "22551",
      "country": "US"
    }
  },
  "tracking_history": [
    {
      "object_created": "2016-07-22T14:36:50.943Z",
      "object_id": "265c7a7c23354da5b87b2bf52656c625",
      "status": "TRANSIT",
      "status_details": "Your shipment has been accepted.",
      "status_date": "2016-07-21T15:33:00Z",
      "location": {
        "city": "Las Vegas",
        "state": "NV",
        "zip": "89101",
        "country": "US"
      }
    },
    ...

    {
      "object_created": "2016-07-23T20:35:26.129Z",
      "object_id": "aab1d7c0559d43ccbba4ff8603089e56",
      "status": "DELIVERED",
      "status_details": "Your shipment has been delivered at the destination mailbox.",
      "status_date": "2016-07-23T13:03:00Z",
      "location": {
        "city": "Spotsylvania",
        "state": "VA",
        "zip": "22551",
        "country": "US"
      }
    }
  ]
}
```

Event Definitions
-----------------

Possible values for `status`, `substatus` and corresponding `action_required` fields are available below. Each field's definition is also available in our [API Reference documentation here:](https://goshippo.com/docs/reference#tracks)

-   PRE_TRANSIT : The label is created but before the package is dropped off or picked up by the carrier.
-   TRANSIT : The package has been scanned by the carrier and is in transit.
-   DELIVERED : The package has been successfully delivered.
-   RETURNED : The package is en route to be returned to the sender, or has been returned successfully.
-   FAILURE : The carrier indicated that there has been an issue with the delivery. This can happen for various reasons and depends on the carrier. This status does not indicate a technical, but a delivery issue.
-   UNKNOWN : The package has not been found via the carrier's tracking system.

| Status | Substatus | Definition | Action Required |
| --- | --- | --- | --- |
| PRE_TRANSIT | information_received | Information about the package received. |  |
| TRANSIT | address_issue | Address information is incorrect. Contact carrier to ensure delivery. | ✓ |
| TRANSIT | contact_carrier | Contact the carrier for more information. | ✓ |
| TRANSIT | delayed | Delivery of package is delayed. |  |
| TRANSIT | delivery_attempted | Delivery of package has been attempted. Contact carrier to ensure delivery. | ✓ |
| TRANSIT | delivery_rescheduled | Delivery of package has been rescheduled. |  |
| TRANSIT | delivery_scheduled | Package is scheduled for delivery. |  |
| TRANSIT | location_inaccessible | Delivery location inaccessible to carrier. Contact carrier to ensure delivery. | ✓ |
| TRANSIT | notice_left | Carrier left notice during attempted delivery. Follow carrier instructions on notice. | ✓ |
| TRANSIT | out_for_delivery | Package is out for delivery. |  |
| TRANSIT | package_accepted | Package has been accepted into the carrier network for delivery. |  |
| TRANSIT | package_arrived | Package has arrived at an intermediate location in the carrier network. |  |
| TRANSIT | package_damaged | Package has been damaged. Contact carrier for more details. | ✓ |
| TRANSIT | package_departed | Package has departed from an intermediate location in the carrier network. |  |
| TRANSIT | package_forwarded | Package has been forwarded. |  |
| TRANSIT | package_held | Package held at carrier location. Contact carrier for more details. | ✓ |
| TRANSIT | package_processed | Package has been processed at an intermediate location. |  |
| TRANSIT | package_processing | Package is processing at an intermediate location in the carrier network. |  |
| TRANSIT | pickup_available | Package is available for pickup at carrier location. | ✓ |
| TRANSIT | reschedule_delivery | Contact carrier to reschedule delivery. | ✓ |
| DELIVERED | delivered | Package has been delivered. |  |
| RETURNED | return_to_sender | Package is to be returned to sender. |  |
| RETURNED | package_unclaimed | Package is unclaimed. | ✓ |
| FAILURE | package_undeliverable | Package is not able to be delivered. | ✓ |
| FAILURE | package_disposed | Package has been disposed. |  |
| FAILURE | package_lost | Package has been lost. Contact carrier for more details. | ✓ |
| UNKNOWN | other | Unrecognized carrier status. |  |

* * * * *

Test Mode
---------

In our Test environment you are able to review our responses with mock tracking numbers. Using your Shippo Test Token, you simply need to have the `carrier` set to "shippo" and the tracking number be one of the following, depending on which tracking event you're testing: `SHIPPO_PRE_TRANSIT`, `SHIPPO_TRANSIT`, `SHIPPO_DELIVERED`, `SHIPPO_RETURNED`, `SHIPPO_FAILURE`, `SHIPPO_UNKNOWN` You can see a JSON example below.

```
{
    "carrier": "shippo",
    "tracking_number": "SHIPPO_TRANSIT"
}
```

```
{
    "messages": [],
    "carrier": "shippo",
    "tracking_number": "SHIPPO_TRANSIT",
    "address_from": {
        "city": "San Francisco",
        "state": "CA",
        "zip": "94103",
        "country": "US"
    },
    "address_to": {
        "city": "Chicago",
        "state": "IL",
        "zip": "60611",
        "country": "US"
    },
    "eta": "2018-08-02T18:49:42.566",
    "original_eta": "2018-08-02T18:49:42.566",
    "servicelevel": {
        "token": "shippo_priority",
        "name": "Priority Mail"
    },
    "metadata": "Shippo test tracking",
    "tracking_status": {
        "object_created": "2018-07-30T18:49:42.586",
        "object_updated": null,
        "object_id": "560f1b9cfe8341a2899e0388f1b9081c",
        "status": "TRANSIT",
        "status_details": "Your shipment has departed from the origin.",
        "status_date": "2018-07-29T16:44:42.586",
        "substatus": null,
        "location": {
            "city": "San Francisco",
            "state": "CA",
            "zip": "94103",
            "country": "US"
        }
    },
    "tracking_history": [
        {
            "object_created": "2018-07-30T18:49:42.589",
            "object_updated": null,
            "object_id": "9a056563ad874a0b9bf79dee321f25f5",
            "status": "UNKNOWN",
            "status_details": "The carrier has received the electronic shipment information.",
            "status_date": "2018-07-28T14:39:42.589",
            "substatus": null,
            "location": {
                "city": "San Francisco",
                "state": "CA",
                "zip": "94103",
                "country": "US"
            }
        },
        {
            "object_created": "2018-07-30T18:49:42.589",
            "object_updated": null,
            "object_id": "82ef073b04ef48368e79b835af788fce",
            "status": "TRANSIT",
            "status_details": "Your shipment has departed from the origin.",
            "status_date": "2018-07-29T16:44:42.589",
            "substatus": null,
            "location": {
                "city": "San Francisco",
                "state": "CA",
                "zip": "94103",
                "country": "US"
            }
        }
    ],
    "transaction": null,
    "test": true
}
```

To see all possible status, substatus and corresponding action_required values, review our [full list](https://goshippo.com/docs/tracking#event-definitions) above.

Track Shippo Shipments
----------------------

For each production label you purchase, Shippo automatically tracks the shipment status through the corresponding carrier's tracking system.

The latest status is accessible via the corresponding Transaction's `tracking_status` field.

Here's a sample Transaction object:

```
{
    "object_state": "VALID",
    "status": "SUCCESS",
    "object_created": "2014-11-29T16:31:19.512Z",
    "object_updated": "2014-11-29T16:31:19.512Z",
    "object_id": "5695ae3a5eda41ba9abdbf347fd545f3",
    "object_owner": "test@goshippo.com",
    "test": false,
    "rate": "693ea14a541f44e090291b929c171d5a",
    "tracking_number": "9102969010383081813033",
    "tracking_status": "DELIVERED",
    "eta": "2014-11-24T00:00:00Z",
    "tracking_url_provider": "https:\/\/tools.usps.com\/go\/TrackConfirmAction_input?origTrackNum=9102969010383081813033",
    "label_url": "https:\/\/shippo-delivery-east.s3.amazonaws.com\/5695ae3a5eda41ba9abdbf347fd545f3.pdf?Signature=AyiitLq2g%2F2R9fjboCTVxi5z7JQ%3D&Expires=1534873886&AWSAccessKeyId=AKIAJGLCC5MYLLWIG42A",
    "commercial_invoice_url": null,
    "messages": [],
    "order": "ca760ef0099040b4a2b7feec827bca88",
    "metadata": "",
    "parcel": "e0de043b2f7f4b6d8e6f23ad69641cc1",
    "billing": {"payments": []}
}
```

Track Individual Shipments
--------------------------

To submit an individual tracking request for a single shipment, including those created outside of Shippo, you can send a GET request to the [Tracking Status endpoint](https://goshippo.com/docs/reference#tracks) with your shipment `carrier` and `tracking_number`.

You can find a [list of all supported carrier tokens here](https://goshippo.com/docs/reference#carriers).

Here's a sample request.

```
curl https://api.goshippo.com/tracks/usps/9205590164917312751089\
    -H "Authorization: ShippoToken <API_TOKEN>"
```

The response is similar to the example above.

* * * * *

Adding Metadata
---------------

You can add `metadata` to the tracking request through a POST request. This is a free form field where any user-specific information can be added. By making a POST request to add metadata, all your existing webhooks will also be connected to the tracked shipment.

The POST request's body needs to consist of your shipment's `tracking_number` and `carrier`:

```
curl https://api.goshippo.com/tracks/\
    -H "Authorization: ShippoToken <API_TOKEN>"\
    -d carrier="usps"\
    -d tracking_number="9205590164917312751089"\
    -d metadata="Order 000123"
```

Carrier Restrictions
--------------------

Some carriers require that you have an account with them in order to track shipments being delivered through them. This is only relevant if you are sending your tracking numbers as a POST to get updates via our webhook (where it is required to have a Shippo account). Otherwise, you simply wouldn't be able to track shipments through the below carriers.

In order to track shipments with the below carriers, you need to add your respective carrier account into your Shippo account:

-   Australia Post
-   Purolator

See [Carrier Accounts](https://goshippo.com/docs/carrier-accounts) for details on adding carrier accounts to your Shippo account.

Tracking Pages
--------------

Tracking pages let retailers share detailed order tracking information through the native look and feel of their own brand. You can [brand your own tracking pages](https://goshippo.com/blog/tracking-pages/) within the Shippo App in three easy steps. Contact our sales team to get started, [here.](https://goshippo.com/contact/sales/)

1.  Log in to our App to upload assets to the Branding and Tracking Page tabs
2.  Publish content and retrieve your base URL from the 'See Live Example' link
3.  Send customers to your pages (include this link in your own emails, for example) with the <baseurl>/<carrier>/<trackingnumber>