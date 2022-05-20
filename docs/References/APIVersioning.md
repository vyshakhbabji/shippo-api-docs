API Versioning
==============

The Shippo API uses versioning to roll out backwards-incompatible changes over time.

About Versioning
----------------

The API version will control the API and webhook behaviors, such as parameters accepted in requests, and response properties. Your account is automatically set to the latest version when you sign up for Shippo.

A new version of the API is released when backwards-incompatible changes are made to the API. To avoid breaking your code, we will never force you to upgrade until you're ready.

We will be releasing backwards-compatible changes without introducing new versions. Your code will be able to handle these changes no matter what version it's on.

Examples of backwards-compatible changes:

-   Adding new API endpoints
-   Adding new optional response attributes to an existing resource
-   Adding new optional request attributes

Changelog and Communication
---------------------------

Backwards-incompatible changes will be documented in the [changelog](https://goshippo.com/docs/changelog) as part of every version release. Documentation will provide in-depth explanations of the changes.

Upgrade Your API Version
------------------------

We recommend staying up-to-date with the current API version to take advantage of latest improvements to the Shippo API.

To see your current version and upgrade to the latest, visit the [API tab](https://apps.goshippo.com/login?next=/user/apikeys/) on the Shippo dashboard.

Versioning of the Shippo API will be released as dates, displayed as: `YYYY-MM-DD`

Test Before Upgrading
---------------------

To test your code under a different API version before committing the change, you can set the API version on a specific request by setting a header with the version you are testing. The version will be set for subsequent requests until it's changed back.

Users can only upgrade their API to the latest version. Once you've upgraded your API version, you cannot roll back to an earlier version. Please make sure to test thoroughly before doing so.

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
    -H "Shippo-API-Version: YYYY-MM-DD"\
    -d '{ ... }'
# now this API request uses the API version "YYYY-MM-DD"
```