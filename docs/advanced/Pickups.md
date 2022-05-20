Pickups
=======

Shippo's pickups endpoint allows you to schedule pickups with USPS and DHL Express for eligible shipments that you have already created.

Creating a Pickup
-----------------

You can create a pickup request by sending a POST request to the pickups endpoint. A pickup typically consists of the following parameters:

-   `carrier_account`: The object ID of your USPS or DHL Express carrier account -- this is a mandatory field. You can retrieve this from your Rate requests or our  /carrier_accounts endpoint.
-   `location`: The following fields may also be included in location
    -   `building_location_type:` where your parcels will be available for pickup (accepted enums are: Front Door, Back Door, Side Door, Knock on Door/Ring Bell, Mail Room, Office, Reception, In/At Mailbox, Security Deck, Shipping Dock, Other)
    -   `building_type`: an optional field to describe the type of building (accepted enums are: apartment, building, department, floor, room, suite) where the pickup is located
    -   `instructions`: pickup instructions for the courier (string). This is a mandatory field if the building_location_type is "Other"
    -   `address`: the pickup address, which includes your name, company name, street address, city, state, zip code, country, phone number, and email address (strings). Special characters (/, &, *, etc.) should not be included in any address element, especially name, company, and email.
-   `transactions`: The transaction object ID(s) for the parcel(s) that need to be picked up. This should include all eligible parcels.
-   `requested_start_time`: The earliest that your parcels will be ready for pickup (UTC time)
-   `requested_end_time`: The latest that your parcels will be available for pickup (UTC time)
-   `metadata`: an optional field for any additional information you might want to add such as the date of fulfillment

```
{
    "carrier_account":"6c51273296864869829b96a80fb13ea1",
    "location":{
            "building_location_type": "Other",
            "building_type": "apartment",
            "instructions": "Behind screen door",
            "address": {
                    "name": "Mrs Hippo",
                    "company": "Hungry Hippos",
                    "street1": "965 Mission St #201",
                    "city": "San Francisco",
                    "state": "CA",
                    "zip": "95122",
                    "country": "US",
                    "phone": "+14159876543",
                    "email": "mrshippo@goshippo.com"
                }
    },
    "transactions": ["7439c279b374494c9a80ca24f59e6fc5"],
   "requested_start_time":"2019-02-18T12:00:00Z",
   "requested_end_time": "2019-02-18T16:00:00Z",
    "metadata": "Customer ID 123456",
    "is_test": false

}
```

The API will respond with the JSON serialized pickups object:

```
{
    "object_created": "2020-05-08T17:09:48.028Z",
    "object_updated": "2020-05-08T17:09:48.884Z",
    "object_id": "e0cefba8a75f401e893db1eb09075efb",
    "carrier_account": "6c51273296864869829b96a80fb13ea1",
    "location": {
        "instructions": "",
        "building_location_type": "Knock on door",
        "building_type": null,
        "address": {
            "object_id": "50ed4a6f0b0d4635b05315c79529798g",
            "name": "Mrs Hippo",
            "company": "Hungry Hippos",
            "street1": "965 Mission St #201",
            "street2": "",
            "city": "San Francisco",
            "state": "CA",
            "zip": "95122",
            "country": "US",
            "phone": "+14159876543",
            "email": "mrshippo@goshippo.com"
        }
    },
    "transactions": [
        "e2aacd6211d347ada7b3d832f87db89h"
    ],
    "requested_start_time": "2020-05-12T19:00:00Z",
    "requested_end_time": "2020-05-12T23:00:00Z",
    "confirmed_start_time": "2020-05-09T12:00:00Z",
    "confirmed_end_time": "2020-05-09T23:59:59.999Z",
    "cancel_by_time": "2020-05-09T08:00:00Z",
    "status": "CONFIRMED",
    "confirmation_code": "WTC310058750",
    "timezone": "US/Pacific",
    "messages": null,
    "metadata": "Customer ID 123456",
    "is_test": false
}
```

Note that your confirmed pickup window will be in the time zone specified in the response, not UTC.

Make sure to take down your `confirmation_code` in the event you need to make changes. USPS may send you a follow-up email with your pickup confirmation code. To edit or cancel a pickup, you will need to contact USPS or DHL Express directly and provide your `confirmation_code`. The ability to edit or cancel a pickup through Shippo may be released in future iterations.

Error Messages
--------------

You may receive the following error messages if you are missing anything from the above fields in your request.

### Address or Address Elements Missing

Your pickup address must be submitted in the request with all elements, including city, zip code, country, and phone number.

```
{
    "address": [
        "This field is required."
    ]
}
```

### Special Characters Used

Special characters, including /, &, *, and others, should not be used within the `address` elements, especially `name`, `company`, and `email`, as some of our carriers do not accept special characters.

```
{
    "messages": [
        "The company name of the pickup address is invalid or missing. Please update the company on the pickup address."
    ]
}
```

### No Eligible Parcels Available for Pickup

At least one eligible USPS or DHL Express transaction object ID must be included in the pickup request. For USPS, only Priority Mail Express, Priority Mail, international, or return service parcels are eligible for a scheduled pickup via API.

```
{
    "messages": [
        "A USPS pickup requires at least one transaction of the following service: Priority Mail Express, Priority Mail, any international service, or any return service. Please update the transactions."
    ],
}
```

### Pickup Already Scheduled for the Given Date and Time (USPS only)

USPS only allows one pickup scheduling request per day. If you have already scheduled a pickup for the given day, you will not be able to request another pickup.

```
{
    "messages": [
        "You have already requested a USPS pickup for today. Please leave any additional USPS packages at your designated pickup location and the carrier will collect them along with your already-scheduled package."
    ],
}
```