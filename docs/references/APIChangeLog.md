API Changelog
=============

We will be documenting all backwards-incompatible changes associated with each new API version here on the changelog. [Learn more about how Shippo API versioning works and upgrade to the latest.](https://goshippo.com/docs/versioning)

Version 2018-02-08
------------------

### Carrier Accounts

For security reasons, the `account_id` and `parameters` fields will be censored in the response objects.

### Tracks

PRE_TRANSIT will be added to the list of possible values in `status`.

### Webhooks

PRE_TRANSIT will be added to the list of possible values in `status` and `tracking_status` for `track_updated` and `transaction_updated` events, respectively.

Version 2017-08-01
------------------

### Webhooks

`Shippo-API-Version` will be included in the headers of all webhook responses to indicate the api version being used.

New events have been added to our webhooks:

-   `transaction_updated` -- will be triggered when a transaction in your account is updated, and will provide the updated transaction.
-   `transaction_created` -- will be triggered when a transaction in your account is created, and will provide the updated transaction.

A new fields have been added in the body of the webhook payload:

-   `event_type` will have one of the following values: `transaction_created`, `transaction_updated`, `track_updated`, `batch_created`, or `batch_updated`.

Please see our [Webhooks](https://goshippo.com/docs/webhooks#webhooks) tutorials for examples of using these new fields and events.

### All Endpoints

The `count` field has been removed. If making a GET request to any endpoint without an `object_id` specified in the URI (i.e. /transactions, /shipments, /addresses) you will no longer see the `count` field in the body of the response.

### Rates

In rate objects, the `days` field has been renamed to `estimated_days` to make it clear that this is an estimate.

### Transactions

The `tracking_history` field has been deprecated from the Transactions object. The Transactions object will still have the most recent `tracking_status` on it, but to get a full tracking history, you would need to subscribe a[webhook](https://goshippo.com/docs/webhooks) to the `tracking_update` event or query the [Tracks](https://goshippo.com/docs/reference#tracks) endpoint.

Version 2017-03-29
------------------

### Major Update: Refactored and Deprecated Fields

In this API version, we've cleaned up many stale attributes across various endpoints in the API to make them more intuitive and true to operational use cases. Please explore the following to see if any changes will affect your implementation.

The changes to our Shipments endpoint for [returns](https://goshippo.com/docs/return-labels) now makes it possible to create return labels without having to reference a previous transaction.

Changes have been made to the following endpoints: [Addresses](https://goshippo.com/docs/reference#addresses), [Shipments](https://goshippo.com/docs/reference#shipments), [Rates](https://goshippo.com/docs/reference#rates), [Transactions](https://goshippo.com/docs/reference#transactions), [Batches](https://goshippo.com/docs/reference#batches), and [Refunds](https://goshippo.com/docs/reference#refunds). For more detailed information, please see the [full API references](https://goshippo.com/docs/reference).

### Addresses

Request:

-   Deprecated:

    -   `object_purpose`

Response:

-   Deprecated:

    -   `ip`
    -   `object_purpose`
    -   `object_state`
    -   `object_source`
-   Added:

    -   `is_complete` --- boolean used to indicate if an address is fully entered, making it possible to use for purchasing a label.
    -   `validation_results` --- object that contains information regarding if an address had been validated or not. Also contains any messages generated during validation. Children keys are `is_valid`(boolean) and `messages`(array).
-   Updated:

    -   `messages` has been moved into new `validation_results` field.

### Shipments

Request:

-   Deprecated:

    -   `object_purpose`
-   Updated:

    -   `submission_date` is now `shipment_date`
    -   `return_of` is now `is_return`, also has been changed to a boolean and is no longer a top-level key and has been moved into extra field. This will flag a shipment to be created as a scan-based label.
    -   `object_status` is now `status`
    -   `insurance_amount` is now `amount`, moved into newly added`insurance` field inside `extra`object.
    -   `insurance_currency` is now `currency`, moved into newly added `insurance` field inside `extra` object.
    -   `insurance_provider` is now `provider`, moved into newly added `insurance` field.
    -   `insurance_content` is now `content`, moved into newly added `insurance` field.
    -   `reference_1` & `reference_2` moved to `extra` field.
    -   `parcel` is now `parcels` and has been changed to an array.
-   Added:
    -   `insurance`, an object within the `extra` object with previous top level and `extra` level insurance fields as keys of the `insurance` object.

Response:

-   Deprecated:

    -   `object_state`
    -   `submission_type`
    -   `object_purpose`
    -   `rates_url`
-   Updated:

    -   `rates_list` is now `rates`
    -   `submission_date` is now `shipment_date`
    -   `return_of` is now `is_return`, also has been changed to a boolean and is no longer a top-level key and has been moved into the `extra` field. This will set the shipment as a return shipment, which makes the label scan-based. See our docs on [Returns](https://goshippo.com/docs/return-labels) for more information.
    -   `object_status` is now `status`
    -   `insurance_amount` is now `amount`, moved into newly added`insurance` field.
    -   `insurance_currency` is now `currency`, moved into newly added `insurance` field.
    -   `insurance_content` is now `content`, moved into newly added `insurance` field.
    -   `insurance_provider` is now `provider`, moved into newly added `insurance` field.
    -   `reference_1` & `reference_2` moved to `extra` field.
    -   `address_to`,`address_from`, `address_return`, and `parcel` are now expanded to include full address or parcel information, in addition to their `object_id`.
    -   `parcel` is now `parcels` and has been changed to an array.
-   Added:

    -   `insurance`, an object within the `extra` object with previous insurance fields as keys of the `insurance` object.

### Rates

Request:

-   Deprecated:

    -   GET on /rates/ will no longer return a list of rates. GET on /rates/{object_id} will still work as indicated.

Response:

-   Deprecated:

    -   `object_state`
    -   `object_purpose`
    -   `object_updated`
    -   `trackable`
    -   `delivery_attempts`
    -   `rates_url`
-   Updated:

    -   `servicelevel_name`, `servicelevel_token`, and `servicelevel_terms` have been renamed to `name`, `token`, and `terms`(respectively) and moved into the newly created `servicelevel`field.
-   Added:

    -   `servicelevel`, object that contains renamed fields: `name`, `token`, and `terms`(previously `servicelevel_name`, `servicelevel_token`, and `servicelevel_terms`)

### Transactions

Response:

-   Deprecated:

    -   `customs_note`
    -   `submission_note`
-   Updated:

    -   `object_state` is now `status`
    -   `rate` field is expanded with full rate details for insta-label calls.

### Manifest

Request:

-   Updated:

    -   `submission_date` is now `shipment_date`

Response:

-   Updated:

    -   `submission_date` is now `shipment_date`

### Batches

Response:

-   Updated:

    -   `object_status` is now `status`

### Refunds

Response:

-   Updated:

    -   `object_status` is now `status`

### Parcel

Request:

-   Updated:

    -   `reference_1` and `reference_2` can be added in the `extra` field of Parcel for multi-parcel shipments. See [Multi-Piece Shipments](https://goshippo.com/docs/multipiece) for more information.

Version 2016-10-25
------------------

### Major Update: New Test Token used to set Shippo to test mode.

You will now be able use all Shippo API functionalities from end-to-end in test mode by authenticating requests with a Test Token. Previously, to test out Shippo, you had to set carrier accounts to test mode. Now all users will have have two different tokens: one for test mode and one for live mode.

[See our detailed documentation on Test Mode.](https://goshippo.com/docs/test-mode)

#### For users continuing to use API version 2014-02-11

You will have the option of using the Test Token while remaining on the older version of the Shippo API.

-   Test Token can be used for test mode without affecting your existing implementation. See our [Test Mode documentation](https://goshippo.com/docs/test-mode) to learn more about functionalities.
-   Functionalities of your existing Private Auth Token will remain the same -- now renamed as "Live Token".
-   All tokens will now begin with `"shippo_live_"` or `"shippo_test_"`. This will not affect any existing implementation with old tokens, however we recommend using the new format going forward.

#### For users upgrading to API version 2016-10-25

Using the Test Token and Live Token will affect how your test and live data are returned and displayed.

Test and Live Data

-   When using the Test Token, you will only be able to create and request test data sets such as addresses, rates, and carriers etc. No live data can be requested or returned.
-   When using the Live Token, you will only be able to create and request live data. No test data can be requested or returned.
-   Shipment object will now return 3 variables for `test`
    -   True -- All Rates returned in Shipment were tests
    -   False -- All Rates returned in Shipment were live
    -   None -- Rates returned in Shipment were both tests and live

The following are some common scenarios.

1.  If you create a Shipment with Live Token using an Address object created with a Test Token, you will receive a 404 response (not found). Vice versa is also true. If you retrieve an object_id created in live mode with a Test Token, you will receive a 403 (bad request) response.
2.  If you request a list of Transaction objects, or Shipment objects with the Test Token, only test Transactions or Shipments will be returned. Vice versa is also true, when making requests in live mode.

Carrier account configuration

When you upgrade to the latest version, the `test` attribute of the carrier object will become read-only.

-   User-owned carrier accounts (where you have plugged in your own carrier credentials) that have been set to `test: true` at the time of upgrade will only appear and work when request with a Test Token.
-   User-owned carrier accounts (where you have plugged in your own carrier credentials) that have been set to `test: false` at the time of upgrade will only appear and work when request with a Live Token.
-   Shippo-owned master accounts [(see carrier capabilities for full list)](https://goshippo.com/docs/carriers) object_ids will appear and be functional when requested with a Test Token or a Live Token.

We recommend setting all carrier accounts to `test: false` [(including on your dashboard)](https://goshippo.com/user/carriers/) before upgrading so that existing carrier accounts are set to live mode. This will help with minimizing changes necessary for the upgrade.

"test" attribute

Certain objects will now be returning a new `"test"` attribute.

-   Transaction object will no longer return the "was_test" attribute, instead it will return "test".
-   Address, Parcel, Refund, Customs Declaration, and Customs Items objects will now return a new "test" attribute.
-   Possible values for "test":
    -   True -- the object was created in test mode
    -   False -- the object was created in live mode
-   Other objects (such as Carrier Accounts and Rates) will keep returning "test" if they did in past.