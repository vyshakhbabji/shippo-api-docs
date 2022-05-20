Carrier Accounts
================

You can create, modify, delete, and specify which shipping carriers you want to use via the Shippo API. This allows you to connect your shipping account to compare and purchase different Rates for each Shipment. Once you add your own account, you will get your negotiated rates from your carrier account.

You can also manually connect to carriers through the [Carriers page](https://goshippo.com/user/carriers/) of your Shippo Dashboard.

Default Carrier Accounts
------------------------

You won't need to sign up for a new carrier account to start testing or shipping on Shippo. By default, you have access to Shippo's discounted master accounts for U.S. outbound shipments to retrieve shipping rates and purchase labels.

If you want to use your own carrier account, continue this tutorial.

Create a Carrier Account Object
-------------------------------

Each carrier has different account properties. For instance, while FedEx requires an account number and a meter number, UPS asks you for a UPS account number, user ID and password. Each Carrier Account object will have a unique `object_id` that you will need to reference in your API calls.

The Carrier Account object can be created (POST), modified or de-activated (PUT), or retrieved (GET). You can also list (GET) all accounts.

Here's how you create a carrier account:

cURL

Ruby

Python

PHP

Node

Java

C#

```
curl https://api.goshippo.com/carrier_accounts/\
    -H "Authorization: ShippoToken <API_TOKEN>"\
    -d carrier="fedex"\
    -d account_id="<YOUR-FEDEX-ACCOUNT-NUMBER>"\
    -d parameters='{"first_name": "<YOUR_FIRST_NAME>", "last_name": "YOUR_LAST_NAME", "phone_number": "<YOUR_PHONE_NUMBER>", "from_address_st": "<YOUR_STREET_ADDRESS>", "from_address_city": "<YOUR_CITY>", "from_address_state": "<YOUR_STATE>", "from_address_zip": "<YOUR_ZIP>", "from_address_country_iso2": "<YOUR_COUNTRY>"}'
```

The API will respond with the JSON serialized carrier account object:

```
{
    "account_id": "<YOUR-FEDEX-ACCOUNT-NUMBER>",
    "active": true,
    "carrier": "fedex",
    "object_id": "b741b99f95e841639b54272834bc478c",
    "object_owner": "shippotle@goshippo.com",
    "parameters": {
        "first_name": "<YOUR_FIRST_NAME>",
        "last_name": "YOUR_LAST_NAME",
        "phone_number": "<YOUR_PHONE_NUMBER>",
        "from_address_st": "<YOUR_STREET_ADDRESS>",
        "from_address_city": "<YOUR_CITY>",
        "from_address_state": "<YOUR_STATE>",
        "from_address_zip": "<YOUR_ZIP>",
        "from_address_country_iso2": "<YOUR_COUNTRY>"
    },
    "test": false,
    "active": true,
    "is_shippo_account": false,
    "metadata": ""
}
```

[See our list of supported carriers and capabilities here ▸](https://goshippo.com/docs/carriers)

Using Carrier Accounts in Shipments
-----------------------------------

You can specify which carrier accounts you want to use on a per-shipment basis by passing in each account's `object_id` in the shipment's `carrier_accounts` field as a list. If you don't specify which accounts to use, Shippo will use all your active accounts for this shipment.

cURL

Ruby

Python

PHP

Node

Java

C#

```
curl https://api.goshippo.com/shipments/\
    -H "Authorization: ShippoToken <API_TOKEN>"\
    -d address_from="d799c2679e644279b59fe661ac8fa488"\
    -d address_to="42236bcf36214f62bcc6d7f12f02a849"\
    -d parcels=["7df2ecf8b4224763ab7c71fae7ec8274"]\
    -d carrier_accounts=["b741b99f95e841639b54272834bc478c", "b741b99f95e841639b54272834bc478c" ]\
    -d async=false
```

Filtering Carrier Accounts
--------------------------

You can filter carrier accounts by `carrier` and `account_id`:

cURL

Ruby

Python

PHP

Node

Java

C#

```
curl https://api.goshippo.com/carrier_accounts/?carrier=fedex\
    -H "Authorization: ShippoToken <API_TOKEN>"\
```

Account Structure by Carrier
----------------------------

Each carrier has its own type of fields. Browse the list below to find the required fields for the carriers you want to use.

FedEx
-----

UPS
---

DHL Express
-----------

DHL eCommerce
-------------

```
{
    "carrier": "dhl_ecommerce",
    "account_id": "dhl_ecommerce", // Custom account identifier
    "parameters": {
        "username": "HipposDontLie!", // DHL eCommerce client ID
        "password": "shipshippo", // DHL eCommerce client secret
        "pickup_no": "123123", // DHL eCommerce pickup number
        "facility_code": "23" // DHL eCommerce facility code
    },
    ...
}
```

Canada Post
-----------

```
{
    "carrier": "canada_post",
    "account_id": "shippo_api", // Canada Post API username
    "parameters": {
        "api_password": "HipposDontLie!", // Canada Post API password
        "customer_number": "413781", // Canada Post customer number
        "contract_id": "910412", // Canada Post contract number (optional)
        "payment_method": "Account" // Payment method for account, 'CreditCard' or 'Account' (optional)
        "use_manifests": true // Indicates that shipments will be linked with a manifest (contract customers only; optional)
    },
    ...
}
```

CouriersPlease
--------------

```
{
    "carrier": "couriersplease",
    "account_id": "123456", // CouriersPlease account number
    "parameters": {
        "api_key": "yourapikey" // CouriersPlease API key
    },
    ...
}
```

Purolator
---------

```
    "carrier": "purolator",
    "account_id": "shippo_purolator", // Purolator account number
    "parameters": {
        "production_key": "345345", // Purolator production key
        "production_key_password": "abcdef" // Purolator production key password
    },
    ...
}
```

Asendia
-------

```
{
    "carrier": "asendia_us",
    "account_id": "123", // Asendia account number
    "parameters": {
        "asendia_user_login": "usertesting",  // Asendia user login
        "asendia_user_password": "1234512345",  // Asendia user password
        "ftp_username": "testtest",
        "ftp_password": "xyz123",
        "company_name": "Your Company Name", // Required (displays on manifest)
        "permit_no": "234"
    },
    ...
}
```

DHL Germany
-----------

```
{
    "carrier": "dhl_germany",
    "account_id": "2222222222", // The first 10 digits of your DHL account number
    "parameters": {
        "business_customer_portal_username": "dhl_shippo", // DHL username for www.dhl-geschaeftskundenportal.de
        "business_customer_portal_password": "HipposDontLie!", // DHL password for www.dhl-geschaeftskundenportal.de
        "default_participation_code": "01", // The last 2 digits of your DHL account number
        "tracking_account": "978346", // DHL tracking account (optional)
        "tracking_password": "HipposAreBack!" // DHL tracking password (optional)
    },
    ...
}
```

GLS France and GLS Germany
--------------------------

```
{
    "carrier": "gls_fr", // gls_fr for france, gls_de for germany
    "account_id": "7384207", // GLS customer ID
    "parameters": {
        "contact_id": "9238702", // GLS contact ID
        "depot": "DE 1680", // GLS depot
        "parcel_number_min": "9500001230", // GLS parcel number minimum
        "parcel_number_cur": "9500001244", // GLS parcel number current
        "parcel_number_max": "9500002300", // GLS parcel number maximum
        "cod_parcel_number_min": "2800001250", // GLS cash-on-delivery parcel number minimum
        "cod_parcel_number_cur": "2800001281", // GLS cash-on-delivery parcel number current
        "cod_parcel_number_max": "2800002900", // GLS cash-on-delivery parcel number maximum
        "depot_customer_id": "3764982" // GLS depot customer ID (optional)
    },
    ...
}
```

GLS US
------

```
{
    "carrier": "gls_us",
    "account_id": "1111", // GLS US account number
    "parameters": {
    	"api_token": "your_account_pw", // GLS US account password
	"username": "your_account_username", // GLS US account username
    },
...
}
```

Mondial Relay
-------------

```
{
    "carrier": "mondialrelay",
    "account_id": "9328271", // Mondial Relay merchant ID
    "parameters": {
        "key": "JBHOS29JH19N7V6SJ89MK10K" // Mondial Relay key
    },
    ...
}
```

RR Donnelley
------------

```
{
  "carrier": "rr_donnelley",
  "account_id": "1111",
  "parameters": {
    "processing_site": "LAX"
    "company_name": "Company"
  },
  ...
}
```

Newgistics
----------

```
{
    "carrier": "newgistics",
    "account_id": "you_api_key", // Newgistics API KEY
    "parameters": {
        "merchant_id": "ABCD",
        "facility_id": "1",
        "induction_site_id": "123",
        "mailer_id": "9328271", // (optional)
        "disposition_rule_set_id": "99"
    },
    ...
}
```

LaserShip
---------

```
{
    "carrier": "lasership",
    "account_id": "01isf7yzmpsbfy02172gyu2ek78", // LaserShip API key
    "parameters": {
        "lasership_apiid": "c27908r07cc20r893270adsc0402", // LaserShip API ID
        "critical_pull_time": "16:00"
    },
    ...
}
```

OnTrac
------

```
{
    "carrier": "ontrac",
    "account_id": "37", // OnTrac account number
    "parameters": {
        "password": "testpass" // OnTrac provided password
    },
    ...
}
```

Australia Post
--------------

```
{
    "carrier": "australia_post",
    "account_id": "123456", // Australia Post account number
    "parameters": {
        "api_key": "yourapikey", // Australia Post API key
        "password": "testpass" // Australia Post password
    },
    ...
}
```

Deutsche Post
-------------

```
{
    "carrier": "deutsche_post",
    "account_id": "test@test.com", // Deutsche Post username
    "parameters": {
        "password": "testpass" // Deutsche Post password
    },
    ...
}
```

Hermes UK
---------

```
{
    "carrier": "hermes_uk",
    "account_id": "hermes_account_1",
    "parameters": {
              "hermes_uk_api_user": "user-name",
              "hermes_uk_api_password": "1234qwerty",
              "hermes_uk_client_id": 123,
              "hermes_uk_client_name": "CompanyName",
              "hermes_uk_parcel_shop_api_user": "ABC1234",
              "hermes_uk_parcel_shop_api_password": "12345678"
               },
    ...
}
```

Sendle
------

```
{
    "carrier": "sendle",
    "account_id": "sendle_account_1",
    "parameters": {
              "sendle_id": "1234",
              "api_key": "123456789",
               },
    ...
}
```

Globegistics
------------

```
{
    "carrier": "globegistics",
    "account_id": "Name of this account", // The name of this account on Shippo
    "parameters": {
              "account_number": "123456", // Globegistics Account Number
              "api_key": "123456789", // Globegistics API Key
               },
    ...
}
```

APC Postal
----------