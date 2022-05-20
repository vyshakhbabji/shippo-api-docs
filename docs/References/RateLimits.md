Rate Limits
===========

Shippo has different rate limits depending on the endpoint and the HTTP verb that is being used (GET, POST, PUT, etc...). Below are the different rate limits defined within Shippo.

**All listed rate limits are per minute. Exceeding the rate limit will give a `429` error.**

Limits
------

| **Endpoint** | POST PUT\
Live / Test | GET(single)\
Live / Test | GET(multiple)\
Live / Test | PUT\
Live / Test | Add/Remove\
Live / Test |
| --- | --- | --- | --- | --- | --- |
|

-   Address
-   Parcel
-   Shipment
-   Rate
-   Transaction
-   Customs Item
-   Customs Declaration
-   Refund
-   Manifest
-   Carrier Account

| 500 / 50 | 4000 / 400 | 50 / 5 | 500 / 50 | --- |
|

-   Batch

| 50 / 5 | 400 / 40 | 50 / 5 | --- | 50 / 5 |
|

-   Tracking

| 500 / 50 | 500 / 50 | --- | --- | --- |

GET Single vs GET Multiple
--------------------------

-   GET **Single** -- `https://api.goshippo.com/endpoint/:object_id` request a specific object from that endpoint using the object's `object_id`.
-   GET **Multiple** -- `https://api.goshippo.com/endpoint/` request a list of objects from a given endpoint. See our [Filtering tutorial](https://goshippo.com/docs/filtering) for details on returning specific results.

Need higher limits?
-------------------

[Contact us](https://goshippo.com/contact/sales/) if you require higher rate limits than those found above.