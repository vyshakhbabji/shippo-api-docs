Manifests and SCAN Forms
========================

Manifests are close-outs of shipping labels of a certain day. Daily manifests are required by some carriers and are meant to be used for proper billing and acceptance of shipments. You can create Manifests with the Shippo SCAN Form and Manifest API.

Create a New Manifest
---------------------

To create a manifest for your shipments, POST to the [Manifest endpoint](https://goshippo.com/docs/reference#manifests) with an array of the Transaction object_ids you're looking to create the Manifest for, along with your `carrier_account`, `shipment_date`, and `address_from` (required for USPS).

cURL

Ruby

Python

PHP

Node

Java

C#

```
curl https://api.goshippo.com/manifests/
    -H "Authorization: ShippoToken shippo_test_831a7a042784f523b95db65444e6e084b636764b"\
    -H "Content-Type: application/json"\
    -d '{
          "carrier_account": "b741b99f95e841639b54272834bc478c",
          "shipment_date": "2014-05-16T23:59:59Z",
          "address_from": "28828839a2b04e208ac2aa4945fbca9a",
          "transactions": [
            "64bba01845ef40d29374032599f22588",
            "c169aa586a844cc49da00d0272b590e1"
            ],
          "async": false
        }'
```

In the response, you will find an array of links to your manifests in the `documents` attribute. Depending on how many shipments you've manifested, you may generate multiple files.

Manifests are always generated in PDFs. For the USPS, one manifest (SCAN Form) can contain up to 500 shipment information, so if you've manifested more than 500 shipments you will receive multiple PDFs.

```
{
    "address_from": "28828839a2b04e208ac2aa4945fbca9a",
    "carrier_account": "b741b99f95e841639b54272834bc478c",
    "documents": [
        "https://shippo-delivery.s3.amazonaws.com/0fadebf6f60c4aca95fa01bcc59c79ae.pdf?Signature=tlQU3RECwdHUQJQadwqg5bAzGFQ%3D&Expires=1402803835&AWSAccessKeyId=AKIAJTHP3LLFMYAWALIA"
    ],
    "object_created": "2014-05-16T03:43:52.765Z",
    "object_id": "0fadebf6f60c4aca95fa01bcc59c79ae",
    "object_owner": "mrhippo@goshippo.com",
    "object_updated": "2014-05-16T03:43:55.445Z",
    "shipment_date": "2014-05-16T23:59:59Z",
    "status": "SUCCESS",
    "transactions": [
        "64bba01845ef40d29374032599f22588",
        "c169aa586a844cc49da00d0272b590e1"
    ]
}
```

Which Carriers Require a Manifest?
----------------------------------

Most carriers, including FedEx, UPS and DHL Express, don't require you to create a manifest. As a certified carrier partner, Shippo automatically manifests packages for you if the carrier supports it. Your labels are pre-scanned and drivers are not required to scan each package individually on pickup.

**USPS*** (optional)*

The USPS manifest is also known as "SCAN Form". A SCAN form is a PDF with a single barcode containing information about all your packages, so that the USPS doesn't need to scan each of your packages individually and all tracking codes are updated immediately.

**Canada Post ***(contract customers only)*

If you have a Contract Account with Canada Post and are shipping more than 50 shipments a day you need to create a manifests to transmit the shipments for billing.

Customers that ship less than 50 daily can usually skip the manifest requirement, but are encouraged to verify with Canada Post.

**Newgistics**

All Newgistics customers are required to manifest their shipments, daily.

When a manifest is requested for Newgistics, Shippo will send a "Client Manifest Information" (CMI) file to Newgistics on your behalf. No physical document for scanning will be returned.

Manifests must be posted by 11pm PST the day before a shipment is picked up for processing. It is recommended that manifests contain full and accurate information for all shipments set to be pickup because it will be used as reference.

To have notifications for your Newgistics shipments emailed to customers, you must already have this feature enabled on your Newgistics account and have customer emails associated with their respective shipments in Shippo.

**Australia Post**

All Australia Post customers are required to manifest their shipments on a daily basis.

**DHL eCommerce**

All DHL eCommerce customers are required to manifest their shipments on a daily basis.

**Purolator**

Manifest capabilities are available for Purolator. It is not required by Purolator, however encouraged for record-keeping purposes.