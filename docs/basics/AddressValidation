Address Validation
==================

In order to prevent failed deliveries and address correction surcharges, validate your addresses through our API before you create labels.

Address Validation Logic
------------------------

The Shippo address validation method verifies an Address by checking whether we have a matching verified deliverable address. The following results are possible:

-   **Address is valid**: The source address is valid and unique. The API will return a new, cleaned address object with the `validation_results` object's field `is_valid` is set to `true`.
-   **Address is invalid**: The address validator has processed the address, but could not find a match. There are three possible reasons: (1) the address doesn't exist, (2) the address isn't deliverable, or (3) the address is too ambiguous. The API will return a new address object with the `validation_results` object's `is_valid` field set to `false`. The API also returns messages indicating why the validation failed.

It is also worth noting that if the address object you created is missing information necessary for creating a shipment, the `is_complete` field will be `false`.

The address validator also returns whether the address is residential or commercial via the `is_residential` flag. If the residential/commercial type could not be determined, the value will be null.

How to Validate Addresses
-------------------------

Validating addresses is easy. In your Address POST request, add a field `validate` set to `true`:

cURL

Ruby

Python

PHP

Node

Java

C#

```
curl https://api.goshippo.com/addresses/\
    -H "Authorization: ShippoToken <API_TOKEN>"\
    -d name="Shawn Ippotle"\
    -d company="Shippo"\
    -d street1="215 Clayton St."\
    -d city="San Francisco"\
    -d state="CA"\
    -d zip=94117\
    -d country="US"\
    -d email="shippotle@goshippo.com"\
    -d validate=true
```

You will get a response in the following format:

```
{
    "object_created": "2015-03-30T23:47:11.574Z",
    "object_updated": "2015-03-30T23:47:11.596Z",
    "object_id": "67183b2e81e9421f894bfbcdc4236b16",
    "is_complete": false,
    "validation_results": {
        "is_valid": false,
        "messages": [
            {
                "source": "USPS",
                "code": "Address Not Found",
                "type": "address_error",
                "text": "The address as submitted could not be found. Please check for excessive abbreviations in the street address line or in the City name."
            }
        ]
    },
    "object_owner": "shippotle@goshippo.com",
    "name": "Shawn Ippotle",
    "company": "Shippo",
    "street_no": "",
    "street1": "215 HIPPO ST.",
    "street2": "",
    "city": "SAN FRANCISCO",
    "state": "CA",
    "zip": "94107",
    "country": "US",
    "phone": "",

```

Validate Existing Address Objects
---------------------------------

To validate existing addresses, simply call the validate endpoint at https://api.goshippo.com/addresses/<address-object-id>/validate/:

cURL

Ruby

Python

PHP

Node

Java

C#

```
curl https://api.goshippo.com/addresses/d799c2679e644279b59fe661ac8fa488/validate/\
    -H "Authorization: ShippoToken <API_TOKEN>"
```

This call returns the same response JSON as the sample response above.

* * * * *

Global Address Validation
-------------------------

The way to validate global addresses (non-U.S.) would operate the same as U.S. addresses. The key difference that you'll notice is in the response that you get back. Global address validation has additional messages for specifying the precision of the address that is returned (i.e. street level, neighborhood level, state level, rooftop level, etc.).

If you attempt to validate a non-U.S. address using your Test API token, we will simply pass back the address information without performing any validation. Please be aware that if you simply switch from using your Test token to using your Live token, you *will* incur charges for using your Live token for validating Global addresses.

```
{
    "object_created": "2017-07-26T17:52:37.305Z",
    "object_updated": "2017-07-26T17:52:37.351Z",
    "object_id": "b7f9709df3914d1ca6efe4c30e7b0572",
    "is_complete": true,
    "validation_results": {
        "is_valid": true,
        "messages": [
            {
                "source": "Shippo Address Validator",
                "type": "address_correction",
                "code": "administrative_area_change",
                "text": "The administrative area (state or province) was added or changed."
            },
            {
                "source": "Shippo Address Validator",
                "code": "geocoded_rooftop",
                "text": "The record was geocoded down to rooftop level, meaning the point is within the property boundaries (most often the center)."
            },
            {
                "source": "Shippo Address Validator",
                "code": "premises_full",
                "text": "The address has been verified to the Premise (House or Building) Level, which is the highest level possible with the reference data."
            }
        ]
    },
    "object_owner": "hippo@goshippo.com",
    "name": "Hippo Shippo",
    "company": "Shippo",
    "street_no": "2",
    "street1": "Unter den Linden",
    "street2": "",
    "street3": "",
    "city": "Berlin",
    "state": "",
    "zip": "10117",
    "country": "DE",
    "longitude": "13.39751",
    "latitude": "52.51785",
    "phone": "14151234567",
    "email": "hippo@goshippo.com",
    "is_residential": null,
    "metadata": "",
    "test": false
}
```

### Messages

The messaging structure for global addresses will give details about what was changed and the specificity of your results. You will find these messages in the `messages` field which is an array comprised of all messages returned in processing your address validation request.

-   **type**: this field contains a token that you can use for understanding the type of message that was created. Below is a comprehensive list of the message types that can be returned:
    -   address_error
    -   address_warning
    -   address_correction
    -   geocode_level
    -   geocode_error
    -   service_error
-   **code**: the code further specifies the type of warning, error or message. Some examples are:
    -   administrative_area_change
    -   geocoded_rooftop
    -   premises_full
-   **text**: this field contains a text description of the message. Below are some examples of the text that will accompany the above message codes.
    -   *The address could not be verified at least up to the postal code level.*
    -   *Street could not be matched to a unique street name. Please add more details to the street name.*
    -   *The address matched multiple records, please enter more information to locate a unique address.*
-   **source**: this field describes where the message is coming from, which is always set to"Shippo Address Validator".

```
{
  ...
  "is_complete": true,
  "validation_results": {
    "is_valid": true,
    "messages": [
      {
        "source": "Shippo Address Validator",
        "type": "address_correction",
        "code": "administrative_area_change",
        "text": "The administrative area (state or province) was added or changed."
      },
      {
        "source": "Shippo Address Validator",
        "type": "geocode_level",
        "code": "geocoded_rooftop",
        "text": "The record was geocoded down to rooftop level, meaning the point is within the property boundaries (most often the center)."
      },
      {
        "source": "Shippo Address Validator",
        "type": "address_warning",
        "code": "premises_full",
        "text": "The address has been verified to the Premise (House or Building) Level, which is the highest level possible with the reference data."
      }
    ]
  },
  "object_owner": "hippo@goshippo.com",
  ...
}
```

* * * * *

Bypass Address Validation for Label Purchase
--------------------------------------------

When purchasing shipping labels you can bypass the carrier's built-in address validation of some carriers by setting the Shipment object's `extra` field `bypass_address_validation` to `true`:

```
{
  /* insert other required shipment fields */
  "extra": {
    "bypass_address_validation": true
  }
}
```