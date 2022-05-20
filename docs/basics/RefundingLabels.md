Refunding Labels
================

You can claim reimbursements for any successfully created but unused shipping labels -- including the 5 cent Shippo label fee.

Note: For USPS labels, Shippo will automatically reimburse you for any unused labels 30 days after it's creation.

Create a refund
---------------

To refund a shipping label, POST the Transaction object_id of the label to the [Refund Endpoint.](https://goshippo.com/docs/reference#refunds)

cURL

Ruby

Python

PHP

Node

Java

C#

```
curl  https://api.goshippo.com/refunds/\
    -H "Authorization: ShippoToken <API_Token>"\
    -d transaction="4503427478ea45a899e9b54abc4c5803"	
```

At the moment, you may only request refund for one shipping label at a time. However, remember that Shippo will automatically reimburse you for unused labels 30 days after its creation.

The API will return the following response:

```
{
    "object_created": "2014-04-21T07:12:41.044Z",
    "object_id": "bd7b8379a2e847bcb0818125943dde5d",
    "object_owner": "shippotle@goshippo.com",
    "object_updated": "2014-04-21T07:12:41.045Z",
    "status": "QUEUED",
    "transaction": "35ed59f23a514ecfa2faeaed93a00086"
}
```

Possible statuses include:

-   QUEUED: The request is being processed by Shippo.
-   PENDING: Waiting for more tracking data from carriers, this could take up to 14 days.
-   ERROR: Refund rejected, the shipment was found to be used.
-   SUCCESS: Refund accepted, a negative line item will be added to your next Shippo invoice.

Information about refunds
-------------------------

-   Refunds include the cost of the shipping label, and the Shippo label fee.
-   Once a refund has been claimed, you cannot use the shipping label for sending packages -- it will be rejected.
-   Refunds can take several days to be processed. This is because it can take days for tracking data to come back and confirm that the label has not been used.
-   You will see refunds appear as a negative line item in your next Shippo invoice.
-   Although some carriers don't charge you until a package is scanned into their system, you would still need to refund an unused shipping label in Shippo to refund the Shippo label fee and any insurance that you had purchased through Shippo.